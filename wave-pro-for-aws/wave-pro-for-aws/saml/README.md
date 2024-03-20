# SAMLベースのフェデレーション認証

### 機能概要

SAMLベースのフェデレーション認証がサポートされ、WavePROへログインする際にSSO（Single Sign on）を使ってログインをすることができるようになりました。

この機能により従来のパスワード認証によるアクセスに比べ、管理者側での集権的な管理が可能になり、セキュリティを高めることができます。

### 設定手順

今回は Okta を例に説明していますが、Okta, Auth0, Google Workspace などのSAML 2.0 ベースIdP (ID プロバイダ) に対応しています。

SP (サービスプロバイダー) とは、ここではWavePROを指します。

今回はWave PRO のルートユーザーでログインする場合の設定を記載しています。サブユーザーを使用する場合は[こちら](subuser.md)のドキュメントをご覧ください。

#### 1. OktaのAdminアカウントにログインする

![](https://lh6.googleusercontent.com/Uy5ljABV91COgxWZX72869XMHvRx\_BjaX8dzjhqHjPSg-wPC45apSs4\_14ALmyP31AOK31b0z148StseO\_tDkMi5T9rYJOIUCWSboR\_PsMWIR7kzeRMHQrUQL7PbXx3LPK0e-cKAvsEs2SekI6ii02E)

#### 2.  \[Applications]を選択し、\[Create App Integration] をクリック

![](https://lh3.googleusercontent.com/jaJQzadOmX6dMr4m4oHwKGAlJF2DOLN\_q2ksbcQVevtpl52FLt8xe2tPrTv6oPQKgsLbYUY4beEHV89YdzuV7\_qfCeTQZOlWYnQBVlvODonDxvrXVO8hvEgt-LQBpFD-kFSk-1kBTfb03fK0aNehnu8)

#### 3. SAML 2.0 を選択し Next をクリック

![](https://lh3.googleusercontent.com/aPLMh1l89It\_dkBVAml8zu63zKoEasJlVI-ifK\_WPQpdC0\_K2ZoLqL9m71S\_ZE1lWiiVHlUALDry8amfwIS6Uej2RcpGCNekA9auLdu9wDS9wmJr51NXshfMFt6kpJsfLpAiJ9JthiDk79sNEOMzhQ4)\


#### 4. Create SAML Integrationに必要事項を記入

<mark style="background-color:yellow;">Single sign-on URL:</mark> [<mark style="background-color:yellow;">https://login.alphaus.cloud/wavepro/saml</mark>](https://login.alphaus.cloud/wavepro/saml)\ <mark style="background-color:yellow;">Audience URI (SP Entity ID) :</mark> [<mark style="background-color:yellow;">https://login.alphaus.cloud/wavepro/sam</mark>](https://login.alphaus.cloud/wavepro/saml)<mark style="background-color:yellow;">l</mark>

![](https://lh3.googleusercontent.com/pGyaM9S\_tlS1-7B2GUU-Kxt0mXE4YHuzyuiQthwCu\_drHadjye2VDj\_o2Kf2shfsCRGwQqMZUZ7nMkGK8aEJ8WG0l\_xNjyIOSXXh0mAFwJb8cDBQOh0ciqVuS3t761DXUHtfY3UsauWDrvIYELTSgeo)

![](https://lh5.googleusercontent.com/LCnuI-NA33Q--iX51iq2ORs2ryQmUHbxuUEAxtV4Kbds0RQVnnfgIokyYitVuyFyeyuWmTtPw0AwVytLYqCCVlaasJIAEVV6VQ2hyZOiI07B5nbVd6jte9eohITLSdfZDl2WRLZfAJmB\_hrEDTZIoko)\


Attribute statements:

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/IDPID</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.waveproIdpId</mark>

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/Profiles</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.waveproProfiles</mark>

<mark style="background-color:yellow;">Name: https://app.alphaus.cloud/wavepro/SAML/Attributes/SessionName</mark>\ <mark style="background-color:yellow;">Name format: leave default (Unspecified)</mark>\ <mark style="background-color:yellow;">Value: user.email</mark>

![](https://lh3.googleusercontent.com/qhDu-6cdCKo4AQ2p0BrYMRjg2ae9jFa4DWZiXSWJTiAkg0wCkA-LobtXJS1x0C6lW20uVDcUh8PTngvcttyKIPEHl7peq1zndM2u6uQAAaxxcRx1pTkEMff-OhkC1kaXnvcJFTjK8ImPOx-PgAFxd-s)

![](https://lh6.googleusercontent.com/d-in9nNHWuj4SG9xn\_kEy5cglN7\_REkhWSyhMdqHr1rx2ZSjTjd9McDrvhcmwmecZ1Zsk9yqyutYHTmOjuz89TFnGlGgjDLovsTtHbj9wtjqceqz\_MDFvT7M1sfHmRfaGNYB2wM98SohMKvt5wQ0ciA)

#### 5. SAML Signing Certificatesを取得する

Applications > 作成したアプリケーション（今回はWavePROSSOIdp）＞Sign Onタブ

ページ下部のSAML Signing Certificates のActionsタブから View IdP metadata を選択\
![](https://lh6.googleusercontent.com/AIyRKFoaGkkhKoNkY6sHoXZ8tN50pGN1OUfwhCsXn8RP5nqBZLH6eDrQSP9bk8ZGrrhoSmpZi4tCRqUfQAYAYiySBk-CHi8cB3RWjOwNeQkxCb59x4CFm-S3QHgNFD4fLqvcJxFeeqtKwcEFrM\_ZeYk)

別画面で開いた情報を.xml の拡張子で保存する

![](https://lh5.googleusercontent.com/unJSV7shN\_8BGeYxJlK7IWJMySqTxLFLqGp4nxxNxsi9Yn1YUSnMvNnJp-5AlVeVnLxCgfzj26jhwXRKVJAvr1jWmrxCaFuPREUH5u3QCUaF4OWTQNdv8-GcPo1WR0GtPESFS5eDsm36hiXNAB5Lrg8)

#### 6. Wave PRO にIdP設定を追加する

環境設定 > IdP設定 > +IdPを追加 をクリック

![](https://lh3.googleusercontent.com/ZST7PNu3CR-DPOczIGocezO94wy\_LVswDWxgj2Cj5KIlcHLxE0YwNUTR2t4cGiswC659knlRdfz1sCb8XwsJc9uyHlTEf8JXU5uQXs5XZQ8ahzq9DUIP-X1x-YNvGqP0DRTOIIt67Z-7zCz30y\_Z3o0)

さきほどダウンロードしたファイルをアップロード

![](https://lh4.googleusercontent.com/JHjMHJFQrwHWE6mgv9YdNEsQZGKRXdDGA5-pEm11tbb8mJxH5DdgSLw\_oL-yfmaF5khGiWnDMlpgGSziYlRjPQhhIHi-GdiEBTeCskZkJTgvK9jrn6trjZQJUJ6Tzo2NS-NqmpLV3YCKf2n3NI\_kgpk)\


#### 7. Profile attribute を追加する

メニューのDirectory > Profile Editorで > User (default) をクリック

![](https://lh4.googleusercontent.com/uIpGMmDH5cni7TgnmvHGLxL1AgWLiqVPlozliPwEsOX8f2stN9\_RIvkJXlYiOnR33lhunW4y1QH2\_zI4u8eEfRKhWy5PYmum3Hj1B\_\_zgll4\_Goz-KszvtJMjKNiIqnUCMeyskzFFMqvqSg-rurPRP4)\


Add Attribute から以下のattributeを追加する\
\
<mark style="background-color:yellow;">Display name: waveproIdpId（他の名前でも可）</mark>\ <mark style="background-color:yellow;">Variable name: waveproIdpId</mark>

<mark style="background-color:yellow;">Display name: waveproProfiles（他の名前でも可）</mark>\ <mark style="background-color:yellow;">Variable name: waveproProfiles</mark>

![](https://lh3.googleusercontent.com/y-SWx60-0BvdefeEb\_B773WxJYZ-rE\_Ln37TH9ypn4S\_StoYYFgldjef\_EClCfMKhajFheWRxCGHNM8Vr7QiPNQcs3LmQqOGPDjmFyhlgb8ygChmpMaWrlKllcl5bswBhbjfvzrd29wq83Y1EyQblvE)

![](https://lh5.googleusercontent.com/uuWGuuMPo5Xdce89Pk5fkZHU0SJa6QnAfBdaTZ4\_8oaWtMhDD9ix0AFA-vMciPgF1D5Gyph6siGuKVE43xWU5DLhF3N1WCQLMyDSJoZCgX2CyD90S3naPjEtCAN2NvigC21MC8uO54BRSwv3apXgCJM)



#### 8. Okta でユーザー設定をする

Directory > People > Add person よりユーザーを追加する

![](https://lh6.googleusercontent.com/3Q6TNn4fgMnbHJrUhLtN89bQgIE\_rgcKt8WyTcRH1yao0o3z68DsptRJ\_\_OVZ8bWPxAAjDfM7BJYsUEZgdKuFnknWcSZ23qmPPIsEmkNhppOai2LMO0HbTPgbJCvLtbBbjHj9Wh1weexKCbxTp5SZTA)\


#### 9. アプリケーションにユーザーをアサインする

Applications > Wave PROのアプリケーション > Assignments > Assignボタンより先ほど作成したアカウントを追加する\


![](https://lh4.googleusercontent.com/MvYzVFZOL-zWPFU0cqilmEByKD6HVcrtcvJOJD7h0xe7u74FCPAoVPqCguX5nEXjlskaXuKyqGaIspxtJnAKmPy1cjlrp0BM7Xn4eTVncRn7\_vh2wAKE7jlxwxaHkyAOofcFJyCxhBlPNA97NP0LD50)

#### 10. ユーザー情報をアップデートする

Directory > People > ユーザー選択 > Profile > Edit をクリック

![](https://lh5.googleusercontent.com/GerlCqUuUXA0jVqDpDwV-E8FxoZKiPBpc\_PhNTdkvqE73reMwUBRTVi5tdWneZi3y2Ym7OIk7TYzVrvq2GTUq7lqjraPDg7uddtOdEvdD5zYamLNMfBs7csWi8aYSAWp3-cCI7ROoLD276mNAXVNbYs)

#### 11. AttributesのwaveproIdpId とwaveproProfilesをアップデートする

![](https://lh4.googleusercontent.com/86XXtZOhtH3-3h6B4hO050Ygq3Bh9FNMdwQhQBZQQTFP3huRNzq\_M3xPRjhs-8fN\_WpFlCFXCQYkbex5Do9Rrq5Ss59DbaBNOGpHP5ZjIK3KgwuuIYHoSy6zyDwuZTaI582hQ\_aVif\_YWR42yvK7lJ8)

**waveproIdpId** = Wave PRO の環境設定 > IdP設定で確認できるID

![](https://lh5.googleusercontent.com/GCbr90nmGhG6FCOt57uJbLOUHafHIAXGdj4VUEZQhIxCXD-4jjrpDLjPSOitzkKjnbMf8AVlOPsoZvqm15iQI7ZPCR7jaTMXjjLy\_Wp1eAae0p-v4N25r0Ex5\_YSBLIoX22ls3uTQ-Y2X59O0RPJ9ow)

**waveproProfiles** = \[Wave ID]:wave/\[RBACで設定しているwave権限],user/\[RBACで設定しているuser権限],role/\[RBACで設定しているrole権限]

▼Wave IDの確認画面

![](https://lh6.googleusercontent.com/3Ino1wv5bH2r5QFHg-ATIiWl22C6\_Z9T\_eLXeMojqODcb01L1Qrjw8pR2x3MDl-3lElMawe7INNyC8YmPkPoNTHZIXTyl-A-ogofPOUIWIFdwzlyqzLCUBCl6m6X1hd9c-dFpHgRoYpFb1AxLmFnM7M)\


▼\[RBACで設定しているwave権限],\[RBACで設定しているuser権限],\[RBACで設定しているrole権限]の確認画面

Wave PROの環境設定 > サブユーザーの管理 > サブユーザー管理画面を開く で遷移した先で作成済みのAdmin権限のWAVE、RBAC、USERの内容を記載する

![](https://lh4.googleusercontent.com/1B9hy2UWJtDMTq\_4UyR1s\_bXx2DKuy5TCzxRnoJ-WUZIVVI70ihcRqB2rhXIIlD2fR2AHh-up2qLFtFITBF7zm4KbRL9KQngDOhH7EG4FymD03\_wIONxc1ecke1bAZFZyIx2jVvmrC4MUryzJn3Z7k4)

#### &#x20;10. ログインの確認

設定は以上です。

メニューのApplications > 作成したWave PROアプリケーション > General > ページ下部のEmbed Link を新規のウィンドウにコピーペーストしWave PROにアクセスできれば正しく設定できています。

![](https://lh3.googleusercontent.com/riKg7t6X7VyIv0AzCXHREYxdq5HtbcRqf1Zm2cjKYWuAi3xw8D4j1vYeZPy0QVHG\_a97wTLJwPRbvfMyOoYiyoN05Zo8KHe1DVN2zEOfWGRB6-nPNLGosMLoCX9IY3ullCnKLTwowXcyYQHHoPo9tDY)

![](../../../.gitbook/assets/Untitled\_png.png)

