---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Components: Native Shell

Linked from: [[2026-02-27-bfloat-ide-component-map]]

## Scope
- Total files: 7
- Source root(s): packages/desktop/src-tauri

## Inventory

### `packages/desktop/src-tauri/src/cli.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Direct dependencies (external): `futures::{Stream, StreamExt, future}`, `process_wrap::tokio::CommandWrap`, `process_wrap::tokio::ProcessGroup`, `process_wrap::tokio::{JobObject, KillOnDrop}`
- Used by: 0 file(s)

### `packages/desktop/src-tauri/src/constants.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Direct dependencies (external): `tauri_plugin_window_state::StateFlags`
- Used by: 0 file(s)

### `packages/desktop/src-tauri/src/lib.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Direct dependencies (external): `crate::cli::CommandChild`, `futures::{
    FutureExt,
    future::{self, Shared},
}`, `std::{
    net::TcpListener,
    sync::{Arc, Mutex},
    time::Duration,
}`, `tauri::{AppHandle, Manager, RunEvent, State, ipc::Channel}`
- Used by: 0 file(s)

### `packages/desktop/src-tauri/src/main.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Used by: 0 file(s)

### `packages/desktop/src-tauri/src/server.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Direct dependencies (external): `std::time::{Duration, Instant}`, `tauri::AppHandle`, `tauri_plugin_dialog::{DialogExt, MessageDialogButtons, MessageDialogResult}`, `tauri_plugin_store::StoreExt`
- Used by: 0 file(s)

### `packages/desktop/src-tauri/src/window_commands.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Direct dependencies (external): `tauri::{AppHandle, Manager, window::Color}`, `tauri_plugin_opener::OpenerExt`, `crate::windows::MainWindow`
- Used by: 0 file(s)

### `packages/desktop/src-tauri/src/windows.rs`
- Responsibility: Native Tauri shell/runtime orchestration module.
- Direct dependencies (external): `crate::constants::{UPDATER_ENABLED, window_state_flags}`, `std::{ops::Deref, time::Duration}`, `tauri::{AppHandle, Manager, Runtime, WebviewUrl, WebviewWindow, WebviewWindowBuilder}`, `tauri_plugin_window_state::AppHandleExt`
- Used by: 0 file(s)

## Related
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-27-bfloat-ide-components-desktop-bridge]]