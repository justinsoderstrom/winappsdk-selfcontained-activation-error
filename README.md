# winappsdk-selfcontained-activation-error
A minimum-code reproduction of Windows App SDK single-instancing bug when in self-contained mode.

## Expected behavior
When enforcing single-instancing behavior in the Windows App SDK, you need to redirect the activation to your application instance as [described here](https://learn.microsoft.com/en-us/windows/apps/windows-app-sdk/migrate-to-windows-app-sdk/guides/applifecycle#instructions-for-c) using the [AppInstance.RedirectActivation](https://learn.microsoft.com/en-us/windows/windows-app-sdk/api/winrt/microsoft.windows.applifecycle.appinstance.redirectactivationtoasync?view=windows-app-sdk-1.2) method. In packaged apps, the app should always receive an [activation event](https://learn.microsoft.com/en-us/windows/apps/windows-app-sdk/applifecycle/applifecycle-rich-activation#activation-details-for-packaged-apps) to the [AppInstance.Activated](https://learn.microsoft.com/en-us/windows/windows-app-sdk/api/winrt/microsoft.windows.applifecycle.appinstance.activated?view=windows-app-sdk-1.2) event handler.

This repo is a packaged, self-contained Windows App SDK project that enforces single-instancing in [Program.cs](https://github.com/justinsoderstrom/winappsdk-selfcontained-activation-error/blob/master/WinAppSDKSelfContainedActivationError/Program.cs) following the [Microsoft provided sample implementation](https://github.com/microsoft/WindowsAppSDK-Samples/blob/main/Samples/AppLifecycle/Instancing/cs-winui-packaged/CsWinUiDesktopInstancing/CsWinUiDesktopInstancing/Program.cs). This works as described above when using the default [framework-dependent](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/deploy-overview) deployment model. However, it does **NOT** work when using the [self-contained](https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/self-contained-deploy/deploy-self-contained-apps) deployment model.

The error that occurs when using the self-contained deployment model is `COMException: Interface not registered (0x80040155)` when using [activation](https://learn.microsoft.com/en-us/windows/windows-app-sdk/api/winrt/microsoft.windows.applifecycle.extendedactivationkind?view=windows-app-sdk-1.2) `Launch` or `Protocol` activation.

## Relevant links
Windows App SDK Issue: https://github.com/microsoft/WindowsAppSDK/issues/3439
