---
title: B2B 사용자에 게 온-프레미스 앱에 대 한 액세스 권한 부여-Azure AD
description: 클라우드 B2B 사용자에 게 Azure AD B2B 공동 작업을 통해 온-프레미스 앱에 대 한 액세스 권한을 부여 하는 방법을 보여 줍니다.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 10/30/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd91d1d2c9f5a4a413f9ea64cfdef649823d0f09
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93131023"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Azure AD의 B2B 사용자에게 온-프레미스 애플리케이션에 대한 액세스 권한 부여

Azure AD(Azure Active Directory) B2B 협업 기능을 사용하여 파트너 조직의 게스트 사용자를 Azure AD로 초대하는 조직의 경우 이제 이러한 B2B 사용자에게 온-프레미스 앱에 대한 액세스를 제공할 수 있습니다. 이러한 온-프레미스 앱은 SAML 기반 인증 또는 KCD(Kerberos 제한 위임)과 함께 IWA(Windows 통합 인증)를 사용할 수 있습니다.

## <a name="access-to-saml-apps"></a>SAML 앱에 대한 액세스

온-프레미스 앱이 SAML 기반 인증을 사용하는 경우 손쉽게 Azure AD B2B 협업 사용자가 Azure Portal을 통해 이러한 앱을 사용할 수 있습니다.

다음을 모두 수행해야 합니다.

- [Saml 기반 Single Sign-On 구성](../manage-apps/configure-saml-single-sign-on.md)에 설명 된 대로 saml을 사용 하 여 앱을 통합 합니다. **로그온 URL** 값에 사용하는 값을 알고 있어야 합니다.
-  Azure AD 애플리케이션 프록시를 사용하여 인증 원본으로 구성된 **Azure Active Directory** 와 함께 온-프레미스 앱을 게시합니다. 지침은 [Azure AD 애플리케이션 프록시를 사용하여 애플리케이션 게시](../manage-apps/application-proxy-add-on-premises-application.md)를 참조하세요. 

   **내부 URL** 설정을 구성하는 경우 비갤러리 애플리케이션 템플릿에 지정된 로그온 URL을 사용합니다. 이 방식으로 사용자는 조직 경계 외부에서 응용 프로그램에 액세스할 수 있습니다. 애플리케이션 프록시는 온-프레미스 앱에 대해 SAML Single Sign-On을 수행합니다.
 
   ![온-프레미스 앱 설정 내부 URL 및 인증 표시](media/hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>IWA 및 KCD 앱에 액세스

B2B 사용자에게 Windows 통합 인증 및 Kerberos 제한 위임을 사용하여 보호되는 온-프레미스 애플리케이션에 대한 액세스 권한을 부여하려면 다음과 같은 구성 요소가 필요합니다.

- **Azure AD 애플리케이션 프록시를 통해 인증** 합니다. B2B 사용자는 온-프레미스 애플리케이션에 인증할 수 있어야 합니다. 이렇게 하려면 Azure AD 애플리케이션 프록시를 통해 온-프레미스 앱을 게시해야 합니다. 자세한 내용은 [자습서: 응용 프로그램 프록시를 통해 원격 액세스를 위한 온-프레미스 응용 프로그램 추가](../manage-apps/application-proxy-add-on-premises-application.md)를 참조 하세요.
- **온-프레미스 디렉터리에서 B2B 사용자 개체를 통해 권한을 부여** 합니다. 애플리케이션은 사용자 액세스 검사를 수행하고 올바른 리소스에 대한 액세스 권한을 부여할 수 있어야 합니다. IWA 및 KCD는 이 권한 부여를 완료하기 위해 온-프레미스 Windows Server Active Directory에 있는 사용자 개체가 필요합니다. [KCD를 사용하는 Single Sign-On 작동 방식](../manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works)에 설명된 대로 애플리케이션 프록시는 이 사용자 개체가 사용자를 가장하고 Kerberos 토큰을 앱에 가져와야 합니다. 

   > [!NOTE]
   > Azure AD 응용 프로그램 프록시를 구성 하는 경우 Windows 통합 인증에 대 한 Single Sign-On 구성 (IWA)에서 **위임 된 로그온 id** 가 **사용자 계정 이름** (기본값)으로 설정 되어 있는지 확인 합니다.

   B2B 사용자 시나리오의 경우 온-프레미스 디렉터리에서 권한 부여에 필요한 게스트 사용자 개체를 만드는 데 사용할 수 있는 두 가지 방법이 있습니다.

   - MIM(Microsoft Identity Manager) 및 Microsoft Graph용 MIM 관리 에이전트. 
   - [PowerShell 스크립트](#create-b2b-guest-user-objects-through-a-script-preview). 스크립트 사용은 MIM이 필요하지 않는 더 간단한 솔루션입니다. 

다음 다이어그램은 B2B 사용자에게 온-프레미스 IWA 및 KCD 앱에 대한 액세스 권한을 부여하기 위해 Azure AD 애플리케이션 프록시와 온-프레미스 디렉터리의 B2B 사용자 개체의 생성이 어떻게 함께 작동하는지에 대한 전체적인 개요를 제공합니다. 다이어그램 아래에 번호가 매겨진 단계가 자세히 설명되어 있습니다.

![MIM 및 B2B 스크립트 솔루션 다이어그램](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  파트너 조직(Fabrikam 테넌트)의 사용자가 Contoso 테넌트에 초대됩니다.
2.  게스트 사용자 개체가 Contoso 테넌트에 만들어집니다(예: guest_fabrikam.com의 UPN을 사용하는 사용자 개체 #EXT#@contoso.onmicrosoft.com).
3.  MIM을 통하거나 B2B PowerShell 스크립트를 통해 Contoso에서 Fabrikam 게스트를 가져옵니다.
4.  Fabrikam 게스트 사용자 개체(게스트#EXT#)의 표현 또는 “공간”이 MIM 또는 B2B PowerShell 스크립트를 통해 온-프레미스 디렉터리에 만들어집니다.
5.  게스트 사용자가 온-프레미스 애플리케이션 app.contoso.com에 액세스합니다.
6.  Kerberos 제한 위임을 사용하여 애플리케이션 프록시를 통해 인증 요청이 승인됩니다. 
7.  게스트 사용자 개체가 로컬로 존재하기 때문에 인증에 성공합니다.

### <a name="lifecycle-management-policies"></a>수명 주기 관리 정책

수명 주기 관리 정책을 통해 온-프레미스 B2B 사용자 개체를 관리할 수 있습니다. 다음은 그 예입니다.

- 애플리케이션 프록시를 인증하는 동안 MFA가 사용되도록 게스트 사용자에 대한 MFA(Multi-Factor Authentication)를 설정할 수 있습니다. 자세한 내용은 [B2B 공동 작업 사용자에 대 한 조건부 액세스](conditional-access.md)를 참조 하세요.
- 클라우드 B2B 사용자에서 수행되는 모든 후원, 액세스 검토, 계정 확인 등은 온-프레미스 사용자에게 적용됩니다. 예를 들어 수명 주기 관리 정책을 통해 클라우드 사용자를 삭제 하는 경우 온-프레미스 사용자도 MIM 동기화 또는 Azure AD Connect 동기화를 통해 삭제 됩니다. 자세한 내용은 [AZURE AD 액세스 검토를 사용 하 여 게스트 액세스 관리](../governance/manage-guest-access-with-access-reviews.md)를 참조 하세요.

### <a name="create-b2b-guest-user-objects-through-mim"></a>MIM을 통해 B2B 게스트 사용자 개체 만들기

MIM 2016 서비스 팩 1 및 Microsoft Graph용 MIM 관리 에이전트를 사용하여 온-프레미스 디렉터리에 게스트 사용자 개체를 만드는 방법에 관한 자세한 내용은 [Azure 애플리케이션 프록시를 사용하여 MIM(Microsoft Identity Manager) 2016 SP1과 Azure AD B2B(Business to Business) 협업](/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario)을 참조하세요.

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>스크립트를 통해 B2B 게스트 사용자 개체 만들기(미리 보기)

온-프레미스 Active Directory에서 게스트 사용자 개체를 만드는 데 시작 점으로 사용할 수 있는 PowerShell 샘플 스크립트가 있습니다.

[Microsoft Identity Manager 2016 및 Forefront Identity Manager 2010 r 2 용 커넥터](https://www.microsoft.com/download/details.aspx?id=51495)에서 스크립트 및 추가 정보 파일을 다운로드할 수 있습니다. 다운로드 패키지에서 **스크립트 및 추가 정보를 선택 하 여 AZURE AD B2B 사용자 on-prem.zip파일을 끌어옵니다** .

스크립트를 사용하기 전에 연결된 추가 정보 파일에서 중요한 고려 사항 및 필수 구성 요소를 검토해야 합니다. 또한 스크립트는 샘플로만 사용할 수 있음을 이해합니다. 사용하기 전에 개발 팀 또는 파트너가 스크립트를 사용자 지정하고 검토해야 합니다.

## <a name="license-considerations"></a>라이선스 고려 사항

온-프레미스 앱에 액세스하는 외부 게스트 사용자를 위한 올바른 CAL(클라이언트 액세스 라이선스)가 있어야 합니다. 자세한 내용은 [클라이언트 액세스 라이선스 및 라이선스 관리](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx)의 “외부 커넥터” 섹션을 참조하세요. 특정 라이선스 요구 사항은 Microsoft 담당자 또는 지역 대리점에 문의하세요.

## <a name="next-steps"></a>다음 단계

- [하이브리드 조직에 대한 Azure Active Directory B2B 협업](hybrid-organizations.md)

- Azure AD Connect 개요는 [Azure Active Directory와 온-프레미스 디렉터리 통합](../hybrid/whatis-hybrid-identity.md)을 참조하세요.