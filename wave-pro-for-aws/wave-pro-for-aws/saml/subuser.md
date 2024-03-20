---
description: Wave PROのサブユーザーで認証設定する手順
---

# サブユーザーの設定

### 設定手順

今回は Okta を例に説明していますが、Okta, Auth0, Google Workspace などのSAML 2.0 ベースIdP (ID プロバイダ) に対応しています。

SP (サービスプロバイダー) とは、ここではWavePROを指します。

#### 1. OktaのAdminアカウントにログインする

![](https://lh6.googleusercontent.com/Uy5ljABV91COgxWZX72869XMHvRx\_BjaX8dzjhqHjPSg-wPC45apSs4\_14ALmyP31AOK31b0z148StseO\_tDkMi5T9rYJOIUCWSboR\_PsMWIR7kzeRMHQrUQL7PbXx3LPK0e-cKAvsEs2SekI6ii02E)

#### 2.  \[Applications]を選択し、\[Create App Integration] をクリック

![](https://lh3.googleusercontent.com/jaJQzadOmX6dMr4m4oHwKGAlJF2DOLN\_q2ksbcQVevtpl52FLt8xe2tPrTv6oPQKgsLbYUY4beEHV89YdzuV7\_qfCeTQZOlWYnQBVlvODonDxvrXVO8hvEgt-LQBpFD-kFSk-1kBTfb03fK0aNehnu8)

#### 3. SAML 2.0 を選択し Next をクリック

![](https://lh3.googleusercontent.com/aPLMh1l89It\_dkBVAml8zu63zKoEasJlVI-ifK\_WPQpdC0\_K2ZoLqL9m71S\_ZE1lWiiVHlUALDry8amfwIS6Uej2RcpGCNekA9auLdu9wDS9wmJr51NXshfMFt6kpJsfLpAiJ9JthiDk79sNEOMzhQ4)\


#### 4. Create SAML Integrationに必要事項を記入

<mark style="background-color:yellow;">Single sign-on URL:</mark> [<mark style="background-color:yellow;">https://login.alphaus.cloud/wavepro/saml</mark>](https://login.alphaus.cloud/wavepro/saml)\ <mark style="background-color:yellow;">Audience URI (SP Entity ID) :</mark> [<mark style="background-color:yellow;">https://login.alphaus.cloud/wavepro/sam</mark>](https://login.alphaus.cloud/wavepro/saml)<mark style="background-color:yellow;">l</mark>

![](https://lh3.googleusercontent.com/pGyaM9S\_tlS1-7B2GUU-Kxt0mXE4YHuzyuiQthwCu\_drHadjye2VDj\_o2Kf2shfsCRGwQqMZUZ7nMkGK8aEJ8WG0l\_xNjyIOSXXh0mAFwJb8cDBQOh0ciqVuS3t761DXUHtfY3UsauWDrvIYELTSgeo)

![](https://lh5.googleusercontent.com/LCnuI-NA33Q--iX51iq2ORs2ryQmUHbxuUEAxtV4Kbds0RQVnnfgIokyYitVuyFyeyuWmTtPw0AwVytLYqCCVlaasJIAEVV6VQ2hyZOiI07B5nbVd6jte9eohITLSdfZDl2WRLZfAJmB\_hrEDTZIoko)

![](https://lh5.googleusercontent.com/DVNYoR9n6VNYqADzdWiJXFljTAbq75v8FZnXJF3dspLwIL60T94Zcm\_L23yGn5EcEQ6du08EZOTJWn70kgoGGQ3kJIRUmqw3i8cq0-Mfm-jJ6J5gd52RqFoc7pQ9mgMfBH3-hangMFmN8QwfBohphS8)

Attribute statements:

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/IDPID</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.waveproIdpId</mark>

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/Profiles</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.waveproProfiles</mark>

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/SessionName</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.email</mark>

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/SubUserId</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.subUserId</mark>

![](https://lh6.googleusercontent.com/swpeqG\_KmSgCKuXsjw0PzQxOTrKckttkeKXPs0Nb6LGpxvU3NKXAWvFeADPScdX50fHILuWzsh8qGAMPyCogbSG4ehrDolIoPklNlG5aZ9-3IyuuR0XqxekbE2Vbg8jo6KMA5KUMkEAZmKf5wyr1M88)

#### 5. SAML Signing Certificatesを取得する

Applications > 作成したアプリケーション（今回はWavePROSSOIdp）＞Sign Onタブ

ページ下部のSAML Signing Certificates のActionsタブから View IdP metadata を選択\
![](https://lh4.googleusercontent.com/\_68\_f\_D8JpUv8iF951Qlc0qrufGpbZABuf2poFMTYmyL0fieXf6YfKvCskPhBAI4WZT61u4svnI-DAFJXnDnI-z8-KvrJ4nZJQQ8iKLPv6xNSqyIk1\_7seD2gLkyYnnzYIa8iwojreGA5zT4vl0bOE0)

別画面で開いた情報を保存。この際 .xml の拡張子で保存

![](https://lh4.googleusercontent.com/hI315HxdHoOMVia70ny28-KUaGMwOcPluXUsbzcgmlEt8TqmkigYWih0SqmJIG2yoyBIwmwCLIX\_-UaEQEPOrgGrizhcHBr-3W6UnbVc3-uUz3ZflJ9BdbPt7oj6KxupP1WXXX1BFmlWU9mZsTSNk3g)

#### 6. Wave PRO にIdP設定を追加する

環境設定 > IdP設定 > +IdPを追加 をクリック

![](https://lh3.googleusercontent.com/CZq7uqBVyRQMtQSMbpz2LLOGw45KoI5fevBj\_4rMGzET9NMLAV9wCEZSFlqEUWULUUHrDguVjxbV7CmOt9TZFBbE4Ps3wTsoo-95OUYHWMkZTJuQls3Le2H5Yj7YFIeCcy2VWaxDTzevAodErBQDMSI)

さきほどダウンロードしたファイルをアップロード

![](https://lh5.googleusercontent.com/laC-ofS54vNYMtdzNeow1vpOnxv2kerIgEPILgF6pldo3ClvZim977GZ9kTtA0XGg2BrQ5j-ohHLcR0xwNX50VAqLPnFw3pTb3SpndKo3XmDan3LiFP8VQwx5HUSUBctjtWHfOuLMCgUgsEauCGKCG0)

#### 7. Profile attribute を追加する

メニューのDirectory > Profile Editorで > User (default) をクリック

![](https://lh4.googleusercontent.com/QDUzDJSj4IkaCb59pP95C1eaMmjg\_J5uPcLhVNZFX02Y04SJiUlG9QqkhLZTYjU\_SRfXk6jpFRCTyb\_TdBXvpVFQ\_xq-Z0zAevmgvR-Yz6M-V2TIV4bOMmCVIhvnaDI95ZDfwox0xsB2QzISKzOK354)

* Add Attribute から以下のattributeを追加する

![](https://lh4.googleusercontent.com/1hMKXezpJmMpD4Ch3nAQ4WPx9tJXIpfuzGTeKSd\_Hs-Ph\_02J8zBqt5CTizZFGd9fUEAh65wSJItZCsYJg-S-DMgp8cf6mOEJVVqfK5usy4Y7SCnsb5P1hxqIe0YhPBMF8EshLkOPJyt1pg5xO8cDKY)

以下の3つのAttributeを追加する

<mark style="background-color:yellow;">Display name: waveproIdpId</mark>\ <mark style="background-color:yellow;">Variable name: waveproIdpId</mark>

<mark style="background-color:yellow;">Display name: waveproProfiles</mark>\ <mark style="background-color:yellow;">Variable name: waveproProfiles</mark>

<mark style="background-color:yellow;">Display name: subUserId</mark>\ <mark style="background-color:yellow;">Variable name: subUserId</mark>

![](https://lh4.googleusercontent.com/2XyK4y54C-S17M8PrXsM3RGfkjUgdDNOfx4Dm4AG58vOal4Pa00cz0ERe9LuL9\_WJLMaglDGBuu3kKIL7DFB3oBbA-7GmXYcKZndt7vA1LvsVGaLjGJfsrC0lA8XTEKjcQDg6xkGTCbnPAIMpF2CxgA)

#### 8. Okta でユーザー設定をする

Directory > People > Add person よりユーザーを追加する

![](https://lh6.googleusercontent.com/MAHmwzSuRKFzs0tcINmTUJgMtY0mCLrjJ-k0xA9Gu12egtJqADbpax4O8p62zSW-KhJmsACbWZDdrsnl8o9DWSX-Aas5n7kvIrLOOzroODpDDBbNcuJ8w-Mx1Q4t\_JsMyjZNKMDQfy-jK01ylWRP6zo)

#### 9. アプリケーションにユーザーをアサインする

Applications > Wave PROのアプリケーション > Assignments > Assignボタンより先ほど作成したアカウントを追加する

![](https://lh3.googleusercontent.com/ErmUoPhOKG-ivMHZummEsC97iSQeIN\_uDbj9YIIfIwHtLUq2LfbHeMkAi7OpU8j6M\_8cyo-ApeT-8zbKFQ4f-CcVZDC1xWngEaZnvcUzkyq45urEeC1qfytLI3iAUjwHdTTKz1SaVASjzpJJa\_4n9Fk)

#### 10. ユーザー情報をアップデートする

Directory > People > ユーザー選択 > Profile > Edit をクリック

![](https://lh3.googleusercontent.com/1ln35NI-H3xnXUslYfzeD3X00kbVhV4BpvNQSVZZbTyI3PKlsTf3OlxasbrwjFO3Ks5j0k\_oGm3Luc9020I3FMghyiC47WV\_GCryMaX4ihHbgeJ\_7jXdav\_s4DGbrSxEVbhaS4yofNJAbjtkKRg4\_3k)

#### 11. AttributesのwaveproIdpId とwaveproProfiles、subUserIdをアップデートする

![](https://lh5.googleusercontent.com/WuTm4AqKllRoUOLLA5eJLcAQmt2pJy543KmfQ7VMnZDNb\_ugn5QbUn1aG1ZYiLIhh6g8CV2oxZ1ik60Zfj-ATltzezKS2XwRaTz0HEQVIW8-H0mmMnuLgS1AAOwVvwChbPpofWsBZ0tQ4QtWD49LT-U)

**waveproIdpId** = Wave PRO の環境設定 > IdP設定で確認できるID

![](https://lh3.googleusercontent.com/eK-DegNR0uijI01U88osDju5F4twW2GHNZXCTdXM8ehtv\_\_ZPfq-KDNR4fLzNllzU5y5ITh1tAtgPNpc7n44VOKkfW0Awv\_bbFyFiSaSAx50b7zlyCZ1YNgJXaWnxe4Rmvke0EOCsbUWb\_d7trqe5VU)

**waveproProfiles** = \[Wave ID]:wave/\[RBACで設定しているwave権限],user/\[RBACで設定しているuser権限],role/\[RBACで設定しているrole権限]

▼Wave IDの確認画面

![](https://lh6.googleusercontent.com/B\_c\_dHnD3n3qqW15R1q4VVsNxVVVsT7fFfDV\_wBV6xfEAAbblidaIqINqEQM-D9e6EDPV2pRgYcDXZ5uGK9HqHxeT7QVcBd\_9deYsONhHLkEXBZMfIcZ1dXF8FV5MT-nNXuiu1uVsK38runFYM71YHI)

▼\[RBACで設定しているwave権限],\[RBACで設定しているuser権限],\[RBACで設定しているrole権限]の確認画面

Wave PROの環境設定 > サブユーザーの管理 > サブユーザー管理画面を開く で遷移した先で作成済みのAdmin権限のWAVE、RBAC、USERの内容を記載する

![](https://lh5.googleusercontent.com/-CwS-6ivB9FoHwCS9C3cTvSxEt9tliCNmgsreJMgJZljJvIwYnXRUlMYQYLzl0vF\_cyBAKQ5RRe1DnUWmgK8LqitiVdqb066t6YwUcbnQUv8EFBM00ioSWYP2lh5Opd-e4Mfv\_mO7Dwh\_\_obJHviOB8)\
**subUserId** = サブユーザーでログイン時に利用しているログインID\


#### 10. ログインの確認

設定は以上です。

メニューのApplications > 作成したWave PROアプリケーション > General > ページ下部のEmbed Link を新規のウィンドウにコピーペーストしWave PROにアクセスできれば正しく設定できています。

![](https://lh6.googleusercontent.com/qwfWGJ5Gxk4t16l51rLXTQMZDFGSJVjlb\_EVXFpq9sJBir4bYzUT8tNADW8Ygv8-5ge9WHHlznR-64G9wfA0M8f06n5Oy9Hud\_geWMbAVWYYlnfo30rjKvrLaTZVItL6uqpc4ZflYIPMDzDHiecZ3wM)

\
