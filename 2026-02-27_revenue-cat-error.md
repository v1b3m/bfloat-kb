# RevenueCat Integration JSON Syntax Error

After adding RevenueCat integration to the Focus Timer app, the Expo development server failed to start due to a JSON syntax error in `app.json`.

## Root Cause Analysis

**What Happened:**
During RevenueCat integration, the `expo-build-properties` plugin needed to be configured with iOS deployment target 15.1+ in `app.json`. When using an automated edit tool to replace:

```json
"expo-build-properties"
```

with the proper configuration:

```json
[
  "expo-build-properties",
  {
    "ios": {
      "deploymentTarget": "15.1"
    }
  }
]
```

The edit was malformed and resulted in:

```json
"      [
  \"expo-build-properties\",
  {
    \"ios\": {
      \"deploymentTarget\": \"15.1\"
    }
  }
]"
```

**The Problem:** The entire plugin configuration got wrapped in quotes, making it a string literal instead of a proper JSON array. This caused a `JsonFileError` because the JSON parser couldn't handle the malformed structure.

**The Fix:** Removed the wrapping quotes and extra whitespace to create valid JSON:

```json
[
  "expo-build-properties",
  {
    "ios": {
      "deploymentTarget": "15.1"
    }
  }
]
```

## Error Details

The app fails with the error below in the terminal

```
JsonFileError: Error parsing JSON: {
  "expo": {
    "name": "HelloWorld",
    "slug": "expo-template-default",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "helloworld",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "backgroundColor": "#E6F4FE",
        "foregroundImage": "./assets/images/android-icon-foreground.png",
        "backgroundImage": "./assets/images/android-icon-background.png",
        "monochromeImage": "./assets/images/android-icon-monochrome.png"
      },
      "edgeToEdgeEnabled": true,
      "predictiveBackGestureEnabled": false
    },
    "web": {
      "output": "static",
      "favicon": "./assets/images/favicon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#ffffff",
          "dark": {
            "backgroundColor": "#000000"
          }
        }
      ],
      "      [
        "expo-build-properties",
        {
          "ios": {
            "deploymentTarget": "15.1"
          }
        }
      ]"
    ],
    "experiments": {
      "typedRoutes": true,
      "reactCompiler": true
    }
  }
}

├─ File: /Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/app.json
└─ Cause: SyntaxError: JSON5: invalid character '\n' at 43:0
  41 |       ],
  42 |       "      [
> 43 |         "expo-build-properties",
  44 |         {
  45 |           "ios": {
  46 |             "deploymentTarget": "15.1"
JsonFileError: Error parsing JSON: {
  "expo": {
    "name": "HelloWorld",
    "slug": "expo-template-default",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "helloworld",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "backgroundColor": "#E6F4FE",
        "foregroundImage": "./assets/images/android-icon-foreground.png",
        "backgroundImage": "./assets/images/android-icon-background.png",
        "monochromeImage": "./assets/images/android-icon-monochrome.png"
      },
      "edgeToEdgeEnabled": true,
      "predictiveBackGestureEnabled": false
    },
    "web": {
      "output": "static",
      "favicon": "./assets/images/favicon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#ffffff",
          "dark": {
            "backgroundColor": "#000000"
          }
        }
      ],
      "      [JsonFileError: Error parsing JSON: {
  "expo": {
    "name": "HelloWorld",
    "slug": "expo-template-default",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "helloworld",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "backgroundColor": "#E6F4FE",
        "foregroundImage": "./assets/images/android-icon-foreground.png",
        "backgroundImage": "./assets/images/android-icon-background.png",
        "monochromeImage": "./assets/images/android-icon-monochrome.png"
      },
      "edgeToEdgeEnabled": true,
      "predictiveBackGestureEnabled": false
    },
    "web": {
      "output": "static",
      "favicon": "./assets/images/favicon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#ffffff",
          "dark": {
            "backgroundColor": "#000000"
          }
        }
      ],
      "      [
        "expo-build-properties",
        {
          "ios": {
            "deploymentTarget": "15.1"
          }
        }
      ]"
    ],
    "experiments": {
      "typedRoutes": true,
      "reactCompiler": true
    }
  }
}

├─ File: /Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/app.json
└─ Cause: SyntaxError: JSON5: invalid character '\n' at 43:0
  41 |       ],
  42 |       "      [
> 43 |         "expo-build-properties",
  44 |         {
  45 |           "ios": {
  46 |             "deploymentTarget": "15.1"
JsonFileError: Error parsing JSON: {
  "expo": {
    "name": "HelloWorld",
    "slug": "expo-template-default",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "helloworld",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "backgroundColor": "#E6F4FE",
        "foregroundImage": "./assets/images/android-icon-foreground.png",
        "backgroundImage": "./assets/images/android-icon-background.png",
        "monochromeImage": "./assets/images/android-icon-monochrome.png"
      },
      "edgeToEdgeEnabled": true,
      "predictiveBackGestureEnabled": false
    },
    "web": {
      "output": "static",
      "favicon": "./assets/images/favicon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#ffffff",
          "dark": {
            "backgroundColor": "#000000"
          }
        }
      ],
      "      [
        "expo-build-properties",
        {
          "ios": {
            "deploymentTarget": "15.1"
          }
        }
      ]"
    ],
    "experiments": {
      "typedRoutes": true,
      "reactCompiler": true
    }
  }
}

├─ File: /Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/app.json
└─ Cause: SyntaxError: JSON5: invalid character '\n' at 43:0
  41 |       ],
  42 |       "      [
> 43 |         "expo-build-properties",
  44 |         {
  45 |           "ios": {
  46 |             "deploymentTarget": "15.1"
    at parseJsonString (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/json-file/build/JsonFile.js:198:19)
    at JsonFile.read (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/json-file/build/JsonFile.js:159:12)
    at getStaticConfig (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/config/build/getConfig.js:64:38)
    at getConfig (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/config/build/Config.js:206:85)
    at startAsync (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/expo/node_modules/@expo/cli/build/src/start/startAsync.js:129:68)
    at expoStart (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/expo/node_modules/@expo/cli/build/src/start/index.js:146:12)

        "expo-build-properties",
        {
          "ios": {
            "deploymentTarget": "15.1"
          }
        }
      ]"
    ],
    "experiments": {
      "typedRoutes": true,
      "reactCompiler": true
    }
  }
}

├─ File: /Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/app.json
└─ Cause: SyntaxError: JSON5: invalid character '\n' at 43:0
  41 |       ],
  42 |       "      [
> 43 |         "expo-build-properties",
  44 |         {
  45 |           "ios": {
  46 |             "deploymentTarget": "15.1"
    at parseJsonString (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/json-file/build/JsonFile.js:198:19)
    at JsonFile.read (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/json-file/build/JsonFile.js:159:12)
    at getStaticConfig (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/config/build/getConfig.js:64:38)
    at getConfig (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/@expo/config/build/Config.js:206:85)
    at startAsync (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/expo/node_modules/@expo/cli/build/src/start/startAsync.js:129:68)
    at expoStart (/Users/v1b3m/.bfloat-ide/projects/proj_1772210389605_glc6ooa3z/node_modules/expo/node_modules/@expo/cli/build/src/start/index.js:146:12)

```

## Resolution

**Steps Taken:**
1. Identified the malformed JSON in `app.json` at lines 42-49
2. Removed the wrapping quotes around the `expo-build-properties` configuration
3. Ensured proper JSON array formatting for the plugin
4. Verified the fix by successfully starting the Expo development server

**Key Lesson:** When programmatically editing JSON configuration files, ensure that array/object replacements don't accidentally get stringified. Always validate JSON syntax after automated edits.

**Result:**
- ✅ Expo development server starts successfully
- ✅ RevenueCat integration working properly
- ✅ App.json is now valid JSON with correct plugin configuration

This issue highlights the importance of careful JSON manipulation when adding new dependencies that require configuration changes.
## Prevention Work (2026-02-27)

### Progress
- [x] Added explicit JSON-safe editing and validation requirements to the `add-revenuecat` skill.
- [x] Added runtime guard in chat tool-write application to block invalid `.json` writes before they hit disk.
- [x] Synced RevenueCat skill changes to `bfloat-workbench` for parity.
- [x] Bumped bundled skills version to `4.1.4`.
- [ ] Validate with an end-to-end RevenueCat setup run.

### Decisions
- Use two-layer prevention: skill-level deterministic `app.json` mutation + runtime JSON write guard.
- Fail fast on invalid JSON with a clear surfaced error; do not continue applying the same tool-write batch.
- Guard all AI-written `.json` files, not only RevenueCat `app.json`, to prevent the same class of issue elsewhere.

### Outcome
In progress.
