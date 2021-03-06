---
title: 사용자 지정 정책을 사용하여 리소스 소유자 암호 자격 증명 흐름 구성
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C에서 사용자 지정 정책을 사용하여 ROPC(리소스 소유자 암호 자격 증명) 흐름을 구성하는 방법을 알아봅니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: bf8fe68c28457fd01704762e537fe259a96a6bce
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87116225"
---
# <a name="configure-the-resource-owner-password-credentials-flow-in-azure-active-directory-b2c-using-a-custom-policy"></a>Azure Active Directory B2C에서 사용자 지정 정책을 사용하여 리소스 소유자 암호 자격 증명 흐름 구성

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C(Azure Active Directory B2C)에서 ROPC(리소스 소유자 암호 자격 증명) 흐름은 OAuth 표준 인증 흐름입니다. 이 흐름에서 신뢰 당사자라고도 하는 애플리케이션은 토큰에 유효한 자격 증명을 교환합니다. 자격 증명에는 사용자 ID 및 암호가 포함됩니다. 반환되는 토큰은 ID 토큰, 액세스 토큰 및 새로 고침 토큰입니다.

[!INCLUDE [active-directory-b2c-ropc-notes](../../includes/active-directory-b2c-ropc-notes.md)]

## <a name="prerequisites"></a>필수 구성 요소

[Azure Active Directory B2C에서 사용자 지정 정책을 사용하여 시작](custom-policy-get-started.md)의 단계를 완료합니다.

## <a name="register-an-application"></a>애플리케이션 등록

[!INCLUDE [active-directory-b2c-appreg-ropc](../../includes/active-directory-b2c-appreg-ropc.md)]

##  <a name="create-a-resource-owner-policy"></a>리소스 소유자 정책 만들기

1. *TrustFrameworkExtensions.xml* 파일을 엽니다.
2. 아직 없는 경우 **ClaimsSchema** 요소와 해당 자식 요소를 **BuildingBlocks** 요소 아래의 첫 번째 요소로 추가합니다.

    ```xml
    <ClaimsSchema>
      <ClaimType Id="logonIdentifier">
        <DisplayName>User name or email address that the user can use to sign in</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="resource">
        <DisplayName>The resource parameter passes to the ROPC endpoint</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokenIssuedOnDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokensValidFromDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
    </ClaimsSchema>
    ```

3. **ClaimsSchema** 뒤에 **ClaimsTransformations** 요소와 해당 자식 요소를 **BuildingBlocks** 요소에 추가합니다.

    ```xml
    <ClaimsTransformations>
      <ClaimsTransformation Id="CreateSubjectClaimFromObjectID" TransformationMethod="CreateStringClaim">
        <InputParameters>
          <InputParameter Id="value" DataType="string" Value="Not supported currently. Use oid claim." />
        </InputParameters>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="sub" TransformationClaimType="createdClaim" />
        </OutputClaims>
      </ClaimsTransformation>

      <ClaimsTransformation Id="AssertRefreshTokenIssuedLaterThanValidFromDate" TransformationMethod="AssertDateTimeIsGreaterThan">
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" TransformationClaimType="leftOperand" />
          <InputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" TransformationClaimType="rightOperand" />
        </InputClaims>
        <InputParameters>
          <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
          <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
        </InputParameters>
      </ClaimsTransformation>
    </ClaimsTransformations>
    ```

4. `Local Account SignIn`의 **DisplayName**이 있는 **ClaimsProvider** 요소를 찾아 다음 기술 프로필을 추가합니다.

    ```xml
    <TechnicalProfile Id="ResourceOwnerPasswordCredentials-OAUTH2">
      <DisplayName>Local Account SignIn</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
        <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
        <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
        <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
        <Item Key="response_types">id_token</Item>
        <Item Key="response_mode">query</Item>
        <Item Key="scope">email openid</Item>
        <Item Key="grant_type">password</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="logonIdentifier" PartnerClaimType="username" Required="true" DefaultValue="{OIDC:Username}"/>
        <InputClaim ClaimTypeReferenceId="password" Required="true" DefaultValue="{OIDC:Password}" />
        <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
        <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
        <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
        <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppId" />
        <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppId" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
        <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
      </OutputClaimsTransformations>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
    ```

    **client_ID**의 **DefaultValue**를 필수 조건 자습서에서 만든 ProxyIdentityExperienceFramework 애플리케이션의 애플리케이션 ID로 바꿉니다. **resource_id**의 **DefaultValue**를 필수 조건 자습서에서 만든 IdentityExperienceFramework 애플리케이션의 애플리케이션 ID로 바꿉니다.

5. 다음 **ClaimsProvider** 요소를 기술 프로필과 함께 **ClaimsProviders** 요소에 추가합니다.

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserReadUsingObjectId-CheckRefreshTokenDate">
          <Metadata>
            <Item Key="Operation">Read</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="AssertRefreshTokenIssuedLaterThanValidFromDate" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
          </OutputClaimsTransformations>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-RefreshTokenReadAndSetup">
          <DisplayName>Trustframework Policy Engine Refresh Token Setup Technical Profile</DisplayName>
          <Protocol Name="None" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="JwtIssuer">
          <Metadata>
            <!-- Point to the redeem refresh token user journey-->
            <Item Key="RefreshTokenUserJourneyId">ResourceOwnerPasswordCredentials-RedeemRefreshToken</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

6. **UserJourneys** 요소와 해당 자식 요소를 **TrustFrameworkPolicy** 요소에 추가합니다.

    ```xml
    <UserJourney Id="ResourceOwnerPasswordCredentials">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="ResourceOwnerFlow" TechnicalProfileReferenceId="ResourceOwnerPasswordCredentials-OAUTH2" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    <UserJourney Id="ResourceOwnerPasswordCredentials-RedeemRefreshToken">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="RefreshTokenSetupExchange" TechnicalProfileReferenceId="SM-RefreshTokenReadAndSetup" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="CheckRefreshTokenDateFromAadExchange" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId-CheckRefreshTokenDate" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    ```

7. Azure AD B2C 테넌트의 **사용자 지정 정책** 페이지에서 **업로드 정책**을 선택합니다.
8. **정책이 있는 경우 덮어쓰기**를 사용하도록 설정하고 *TrustFrameworkExtensions.xml* 파일을 찾아서 선택합니다.
9. **업로드**를 클릭합니다.

## <a name="create-a-relying-party-file"></a>신뢰 당사자 파일 만들기

다음으로, 만든 사용자 경험을 시작하는 신뢰 당사자 파일을 업데이트합니다.

1. 작업 디렉터리에 *SignUpOrSignin.xml* 파일의 복사본을 만든 다음, 이름을 *ROPC_Auth.xml*로 바꿉니다.
2. 새 파일을 열고 **TrustFrameworkPolicy**의 **PolicyId** 특성 값을 고유 값으로 변경합니다. 정책 ID는 정책의 이름입니다. 예를 들어 **B2C_1A_ROPC_Auth**입니다.
3. **DefaultUserJourney**의 **ReferenceId** 특성 값을 `ResourceOwnerPasswordCredentials`로 변경합니다.
4. 다음 클레임만 포함되도록 **OutputClaims** 요소를 변경합니다.

    ```xml
    <OutputClaim ClaimTypeReferenceId="sub" />
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="displayName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="givenName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="surname" DefaultValue="" />
    ```

5. Azure AD B2C 테넌트의 **사용자 지정 정책** 페이지에서 **업로드 정책**을 선택합니다.
6. **정책이 있는 경우 덮어쓰기**를 사용하도록 설정하고 *ROPC_Auth.xml* 파일을 찾아서 선택합니다.
7. **업로드**를 클릭합니다.

## <a name="test-the-policy"></a>정책 테스트

선호하는 API 개발 애플리케이션을 사용하여 API 호출을 생성하고, 응답을 검토하여 정책을 디버그합니다. 다음 정보를 POST 요청의 본문으로 사용하여 이 예제와 같은 호출을 생성합니다.

`https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1_ROPC_Auth/oauth2/v2.0/token`

- `<tenant-name>`은 Azure AD B2C 테넌트의 이름으로 바꿉니다.
- `B2C_1A_ROPC_Auth`를 리소스 소유자 암호 자격 증명 정책의 전체 이름으로 바꿉니다.

| 키 | 값 |
| --- | ----- |
| 사용자 이름 | `user-account` |
| password | `password1` |
| grant_type | password |
| scope | openid `application-id` offline_access |
| client_id | `application-id` |
| response_type | 토큰 id_token |

- `user-account`를 테넌트의 사용자 계정 이름으로 바꿉니다.
- `password1`을 사용자 계정의 암호로 바꿉니다.
- `application-id`를 *ROPC_Auth_app* 등록의 애플리케이션 ID로 바꿉니다.
- *Offline_access*는 새로 고침 토큰을 받고 싶은 경우에 사용하는 선택 사항입니다.

실제 POST 요청은 다음 예제와 같이 표시됩니다.

```https
POST /<tenant-name>.onmicrosoft.com/oauth2/v2.0/token?B2C_1_ROPC_Auth HTTP/1.1
Host: <tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

username=contosouser.outlook.com.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```

offline-access의 성공적인 응답은 다음 예제와 같습니다.

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki...",
    "token_type": "Bearer",
    "expires_in": "3600",
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6Ij...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki..."
}
```

## <a name="redeem-a-refresh-token"></a>새로 고침 토큰 교환

여기에 표시된 것과 같은 POST 호출을 생성합니다. 다음 표의 정보를 요청의 본문으로 사용합니다.

`https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1_ROPC_Auth/oauth2/v2.0/token`

- `<tenant-name>`은 Azure AD B2C 테넌트의 이름으로 바꿉니다.
- `B2C_1A_ROPC_Auth`를 리소스 소유자 암호 자격 증명 정책의 전체 이름으로 바꿉니다.

| 키 | 값 |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | `application-id` |
| resource | `application-id` |
| refresh_token | `refresh-token` |

- `application-id`를 *ROPC_Auth_app* 등록의 애플리케이션 ID로 바꿉니다.
- `refresh-token`을 이전 응답에서 다시 전송된 **refresh_token**으로 바꿉니다.

성공적인 응답은 다음 예제와 비슷합니다.

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQndhT...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQn...",
    "token_type": "Bearer",
    "not_before": 1533672990,
    "expires_in": 3600,
    "expires_on": 1533676590,
    "resource": "bef2222d56-552f-4a5b-b90a-1988a7d634c3",
    "id_token_expires_in": 3600,
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiI1MTZmYzA2NS1mZjM2LTRiOTMtYWE1YS1kNmVlZGE3Y2JhYzgiLCJzdWIiOm51bGwsIm5hbWUiOiJEYXZpZE11IiwicHJlZmVycmVkX3VzZXJuYW1lIjpudWxsLCJpZHAiOiJMb2NhbEFjY291bnQifQ",
    "refresh_token": "eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMCIsInppcCI6IkRlZmxhdGUiLCJzZXIiOiIxLjAi...",
    "refresh_token_expires_in": 1209600
}
```

## <a name="use-a-native-sdk-or-app-auth"></a>네이티브 SDK 또는 App-Auth 사용

Azure AD B2C는 공용 클라이언트 리소스 소유자 암호 자격 증명에 대한 OAuth 2.0 표준을 충족하며, 대부분의 클라이언트 SDK와 호환되어야 합니다. 최신 정보는 [모범 사례를 구현하는 OAuth 2.0 및 OpenID Connect용 네이티브 앱 SDK](https://appauth.io/)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Azure Active Directory B2C 사용자 지정 정책 시작 팩](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/source/aadb2c-ief-ropc)에서 이 시나리오의 전체 예제를 참조하세요.
- [토큰 참조](tokens-overview.md)에서 Azure Active Directory B2C에 사용되는 토큰에 대해 자세히 알아봅니다.
