# PlatanoLabs/action-eas-build

Super-speedy Expo builds on GitHub Actions, without needing to pay for EAS. Currently only optimized for iOS.

## Performance

| Runner | First Run | Est. Cost (first) | Second Run (cached) | Est. Cost (cached) |
|--------|-----------|-------------------|---------------------|-------------------|
| `macos-26` | 23m 43s | $1.90 | **12m 10s** | **$0.97** |
| `macos-26-xlarge` | 9m 1s | $1.44 | **5m 5s** | **$0.81** |

> Based on $0.08/min for `macos-26` and $0.16/min for `macos-26-xlarge`

## Usage

```yaml
name: Build
on: push

jobs:
  build:
    runs-on: macos-26
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: 24
      
      - run: npm install 
      
      - uses: PlatanoLabs/action-eas-build@v0.1.0
        id: build
        with:
          expo-token: ${{ secrets.EXPO_TOKEN }}
          platform: ios
          profile: production
      
      - uses: actions/upload-artifact@v4
        with:
          name: ios-build
          path: ${{ steps.build.outputs.build-path }}
```

## Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `expo-token` | ✅ | | Expo authentication token |
| `platform` | ✅ | | `ios` or `android` |
| `profile` | ✅ | | EAS build profile name |
| `disable-ccache` | | `false` | Disable ccache for iOS builds |
| `working-directory` | | `.` | Project directory |

## Outputs

| Name | Description |
|------|-------------|
| `build-path` | Full path to the generated build artifact |
