# Ultra's Unreal Online Subsystem

Ultra's Unreal online subsystem which allows developers to connect the Ultra client to Unreal-based games.

## Installation

1. Add this plugin to your project `YourProjectName/Plugins/OnlineSubsystemUltra`
2. Start Unreal Editor
3. Go to `Edit / Plugins`
4. Search `OnlineSubsystemUltra`
5. Enable the plugin
6. Setup OnlineSubsystemUltra in DefaultEngine.ini (see [Configuration](#configuration) for more details)

## Configuration

To configure Ultra's Unreal Online Subsystem, you need to edit the DefaultEngine.ini:

```ini
[OnlineSubsystem]
DefaultPlatformService=Ultra ; Set OnlineSubsystemUltra as default online subsystem

[OnlineSubsystemUltra]
bEnabled=true
; Authentication
ClientId="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" ; Mandatory - The Client Id given by Ultra
DispatcherUrl="https://api.ultracloud.ultra.io/dispatcherv2" ; Optional - The dispatcher URL (default: https://api.ultracloud.ultra.io/dispatcherv2)
AuthenticationUrl="https://auth.ultra.io/auth/realms/ultraio/protocol/openid-connect" ; Optional - The openid authentication URL (default: https://auth.ultra.io/auth/realms/ultraio/protocol/openid-connect)
bUseBrowser=false ; Optional - If true, the browser will be launched to prompt the user credentials otherwise the 'ApplicationProtocol' will be used to handle the login (default: false)
ApplicationProtocol=ultra ; Optional - If set to "ultra", the Ultra launcher will be called to login the user (default: ultra)
```

## Authentication

```
const IOnlineIdentityPtr IdentityInterface = Online->GetIdentityInterface();
FDelegateHandle Handle;
IdentityInterface->AddOnLoginCompleteDelegate_Handle(UserIndex, FOnLoginCompleteDelegate::CreateLambda([IdentityInterface, Handle](int32 LocalUserNum, bool Success, const FUniqueNetId& UserId, const FString& Error)
{
  IdentityInterface->ClearOnLoginCompleteDelegate_Handle(LocalUserNum, Handle);
  FString Nickname = IdentityInterface->GetPlayerNickname(LocalUserNum);
  // do something
}));
IdentityInterface->AutoLogin(UserIndex);
```
