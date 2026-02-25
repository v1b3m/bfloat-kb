# Northflank Deployment - Scalability Considerations

## Current Architecture

### Project-to-Service Mapping

**Deterministic naming** - No database mapping stored:

```typescript
function generateServiceName(projectId: string): string {
  const shortId = projectId.replace(/-/g, "").substring(0, 8).toLowerCase();
  return `bfloat-${shortId}`;
}
```

- Bfloat project ID → Northflank service name (1:1)
- Same project always maps to same service
- Northflank is source of truth (we just check if service exists)

### Deployment Flow

1. `getServiceByProjectId(projectId)` - Check if service exists
2. If exists → `updateServiceImage()` (push new image, redeploy)
3. If not exists → `createService()` (create new service)

**Result:** Each Bfloat web app = One Northflank deployment service

---

## Scalability Concerns

### 1. Service Name Collisions

| Current | Risk Level |
|---------|------------|
| 8 characters from project ID | Medium |
| Characters: `[a-z0-9]` | 36^8 = ~2.8 trillion theoretical |
| Real-world: project IDs have structure | Lower effective entropy |

**Quick fix:** Increase to 12+ characters
```typescript
const shortId = projectId.replace(/-/g, "").substring(0, 12).toLowerCase();
```

### 2. Single Northflank Project

All services deployed to single project: `bfloat-apps`

**Potential limits:**
- Maximum services per project (check Northflank docs)
- UI manageability with 1000s of services
- Single point of failure

### 3. Cost Structure

Each deployment service has:
- Base compute cost (even when idle)
- Network costs
- Potential minimum billing tiers

**At scale:** 1000 apps × base cost = significant monthly burn

### 4. Resource Isolation

| Pro | Con |
|-----|-----|
| Each app isolated | Higher resource usage |
| Independent scaling | More complex monitoring |
| Crash isolation | Higher infrastructure cost |

---

## Alternative Approaches

### Option 1: Path-Based Routing (Single Service)

**Architecture:**
```
┌─────────────────────────────────────┐
│  bfloat.app (Northflank service)    │
│  ┌──────────┬──────────┬──────────┐ │
│  │ /app1    │ /app2    │ /app3    │ │
│  └──────────┴──────────┴──────────┘ │
└─────────────────────────────────────┘
```

**Pros:**
- Single service = lower cost
- Easier management
- Centralized logging/monitoring

**Cons:**
- Shared runtime (no isolation)
- Single app can affect all others
- Harder to scale individual apps
- More complex routing logic

### Option 2: Per-Customer Projects

**Architecture:**
```
bfloat-customer-1 (NF project)
  └── customer-1-apps

bfloat-customer-2 (NF project)
  └── customer-2-apps
```

**Pros:**
- Better isolation
- Per-customer billing possible
- Failure domain separation

**Cons:**
- More complex setup
- Higher management overhead
- Need to track which customer → which NF project

### Option 3: Northflank Preview Environments

Built for ephemeral, template-based deployments.

**Pros:**
- Designed for short-lived apps
- Auto-cleanup capabilities
- Template-based provisioning

**Cons:**
- May have limitations vs full services
- Need to verify suitability for permanent apps

### Option 4: Hybrid Approach

- **Production apps:** Individual services (current approach)
- **Preview/dev apps:** Path-based routing or preview environments
- **High-volume customers:** Dedicated projects

---

## Recommendations

### Short Term (< 100 apps)

- **Keep current architecture**
- Increase service name to 12+ characters
- Monitor costs closely
- Set up alerting for service count

### Medium Term (100-1000 apps)

- **Implement path-based routing for development/preview apps**
- **Keep 1:1 services for production apps**
- Consider per-customer projects for high-volume users
- Add database tracking of project → service mappings

### Long Term (1000+ apps)

- **Re-architect based on usage patterns**
- Consider multi-region deployment
- Implement auto-scaling and cleanup policies
- Evaluate cost optimization strategies

---

## Open Questions

1. **What's our target scale?** 100s, 1000s, 10000s of apps?
2. **Cost tolerance per app?** What's the maximum we can pay per deployed app?
3. **Isolation requirements?** Do apps need full runtime isolation?
4. **Traffic patterns?** Are most apps idle or do they have consistent traffic?

---

## Resources

- [Northflank: Templates](https://northflank.com/docs/v1/application/templates)
- [Northflank: Preview environments](https://northflank.com/docs/v1/application/release/preview-environments)
- [Northflank: Path-based routing](https://northflank.com/docs/v1/application/domains/use-path-based-routing)

---

*Last updated: January 2025*
