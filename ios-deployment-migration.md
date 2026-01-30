# iOS Deployment: Migration to App Store Connect API Keys

## The Problem

We used PTY stdin injection to parse interactive `eas-cli` prompts and automatically respond. Every time the CLI showed an unexpected prompt, the process got stuck and we had to add another regex pattern.

### Evidence: 21 Regex Patterns Accumulated

```typescript
const PROMPT_PATTERNS = {
  configureProjectPrompt: /Configure this project\?|Existing EAS project found.*Configure/i,
  yesNoPrompt: /\(Y\/n\)|\(y\/N\)/i,
  appleIdPrompt: /Apple ID:/i,
  passwordPrompt: /Password \(for .+?\):/i,
  otpPrompt: /Two-factor Authentication|Enter.*verification.*code|Enter.*2FA.*code|Code:/i,
  loginSuccess: /Logged in|Successfully authenticated|Apple account authenticated/i,
  loginFailed: /Invalid credentials|Authentication failed|Invalid username|incorrect|wrong password/i,
  needsOtp: /Two-factor authentication.*required|2FA.*required|verification code was sent/i,
  selectProviderPrompt: /Select a Provider/i,
  selectTeamPrompt: /Select a Team/i,
  pressAnyKeyPrompt: /Press any key to continue/i,
  credentialsMenu: /What do you want to do\?/i,
  apiKeyMenu: /Use an existing API Key|Add a new API Key|Delete an API Key/i,
  goBackOption: /Go back/i,
  buildCredentialsOption: /Build Credentials/i,
  setupAllOption: /All: Set up all/i,
  exitOption: /Exit/i,
  provisioningProfileCreated: /Provisioning Profile.*created|Successfully created.*provisioning/i,
  credentialsComplete: /All credentials are ready|Credentials.*configured|✔.*credentials/i,
  apiKeyAssigned: /API Key assigned|App Store Connect API Key assigned/i,
  errorPattern: /Error:|error:|FAILURE|failed/i,
}
```

Each pattern represents a time we got stuck and had to debug why.

### Why This Approach Doesn't Scale
- CLI output changes between `eas-cli` versions
- New prompts appear without warning
- Menu navigation with arrow keys (`\x1b[B`) depends on exact menu structure
- No way to predict what prompts will appear

### Why Not Just Use `--non-interactive` Without API Keys?
`--non-interactive` requires App Store Connect credentials to already be configured. Without an API key, EAS CLI needs to prompt for Apple ID/password to authenticate with Apple - which fails in non-interactive mode.

## Solution: App Store Connect API Keys

API keys enable the `--non-interactive` flag, which eliminates all prompts.

| Before | After |
|--------|-------|
| 21+ regex patterns | 0 patterns needed |
| Gets stuck on new prompts | Deterministic `--non-interactive` |
| Constant maintenance | One-time API key setup |

## Implementation
- User creates API key in App Store Connect (one-time)
- Key stored at `~/.bfloat/keys/asc/`
- Build uses `eas build --platform ios --non-interactive --auto-submit`

---

We've decided that users having to manually go to app store connect and create certificates is even more cumbersome. We should aim to make the terminal UI more reliable.

Create a master plan for how we are to handle this app store connect login. Use the AskUserQueston tool so we can answer all manner of nuansces.

---

We need a better way to present information to users

Kudos on the UI that let's uses click instead of typing into a terminal.

However, this is not it

```
[?25l [2K [1G [36m? [1mExisting EAS project found for @bfloat/bfloat-app (id = ...). Configure this project? [22m [90m> [39m [90m(Y/n) [39m]
```

1. We should render this in a user-friendly format without all the terminal stuff
2. The presented question might seem obvious to a dev like me but not to an end user

---

## Issue #1

Should the buttons remain showing after I click them and the answer is registered

```
Yes, use this project (Recommended)
No, create new project
```

This step also involves handling og the "Show original prompt"

## Issue #2

The UI is stuck at

```
> Log in to your Apple Developer account to continue
? Apple ID: ben@bfloat.ai
```

---

The following UI is rendered when I am prompted to sign in to apple

```
Yes, sign in (Recommended)
Skip for now
```

I am presented with multiple options, I hit enter, I am not sure why this even happens as users have already specified their apple account email and password in the previous step (aren't we collecting these for the same reason)

Also, it prompts the UI shows up a second time at this step

```
[?25h> Restoring session /Users/v1b3m/.app-store/auth/ben@bfloat.ai/cookie
```

----

You've failed to handle this completely

The same issues I've mentioned above still presents, I should not have to click sign in to apple at any point. However, when my email shows up, the input UI shows up

"Yes, sign in (Recommended)" shows up a second time

I then get stuck at 

```
Select a Provider >
> Bfloat Inc (121504xxx)
Atem Atem (12...)
```

---

You've made a terrible mistake

The email login is supposed to be "Enter" not "y"

Now apple is asking for

```
? Password (for y): >
```

Once done with the above task, look into these too:
1. Much as the above case is wrong, I also expect the password input to show up by the way