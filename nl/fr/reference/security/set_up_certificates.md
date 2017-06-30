---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-15"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Configuration des certificats
{: #set_up_certificates}

Les certificats sont utilisés pour l'authentification de terminal ou pour remplacer le certificat de serveur {{site.data.keyword.iot_full}} par défaut pour la messagerie MQTT. Tous les terminaux qui ne disposent pas de certificats signés valides se voient refuser l'accès et ne peuvent pas communiquer avec le serveur.

Pour configurer les certificats et l'accès au serveur pour les terminaux, l'opérateur du système enregistre les certificats d'autorité de certification associés et enregistre éventuellement les certificats du serveur de messages dans la plateforme {{site.data.keyword.iot_short_notm}}.

Pour des informations sur l'utilisation des API
de gestion des certificats d'AC et des
certificats de serveur de messagerie, consultez [IBM Watson IoT Platform Authentication and Authorization APIs ![External link icon](../../../../icons/launch-glyph.svg)"Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}.

## Certificats d'autorité de certification
Les certificats d'autorité de certification (AC) permettent à l'organisation de faire confiance aux
certificats de client présents sur les terminaux afin que ces derniers puissent se connecter au serveur.

Si vous ajoutez un certificat d'autorité de certification (AC) ou que vous remplacez le
certificat du serveur de messagerie, tous les terminaux doivent se connecter à l'aide
d'un client MQTT prenant en charge SNI (Server Name Indication) afin que le serveur puisse les authentifier
en utilisant les AC appropriées.


Vous pouvez fixer le niveau de sécurité de connexion en configurant la politique de sécurité de connexion. Pour savoir comment procéder, consultez [Configuration des politiques de sécurité](set_up_policies.html).

## Certificats de client

Les certificats de client individuels (de terminal ou passerelle) sont conservés
sur les terminaux et ne sont pas téléchargés vers la plateforme.
Le certificat de signataire de l'autorité de certification (AC) servant à signer tous les
certificats de terminal et de passerelle est le seul certificat que vous téléchargez vers
la plateforme.
Si vous utilisez des certificats de serveur de messagerie autosignés, vous devez télécharger les
certificats racine et intermédiaires utilisés pour signer le certificat de client (cert.pem).

L'ID du terminal ou de la passerelle doit être entré comme CN (nom commun) ou SubjectAltName dans
le certificat individuel que vous signez avec le certificat de signataire de l'AC pour ce terminal ou cette passerelle.


Pour les terminaux, le format du champ **CN** est `CN=d:devtype:devid`,
et celui du champ **SubjectAltName** est `SubjectAltName=email:d:*devtype:devid*`.
Ici, `email:d` est une constante,
`*devtype*` symbolise le type du terminal et `*devid*` représente l'ID du terminal dans la plateforme.

Pour les passerelles, le format du champ **CN** est `CN=g:typeId:deviceId`
et celui du champ **SubjectAltName** est SubjectAltName=email:g:*devtype:devid*

Remarque : Pour les certificats de terminal ou de passerelle,
l'élément `orgId` ne doit pas être inclus dans le champ **CN** ou **SubjectAltName**.
L'`orgId` doit faire partie des informations SNI fournies par l'implémentation du client lors de la connexion au serveur de messagerie. 

Pour plus d'informations sur les certificats de client, consultez la recette [ Connect Raspberry Pi to IBM Watson IoT Platform using Client side Certificates ![External link icon](../../../../icons/launch-glyph.svg) "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}

## Certificats de serveur de messagerie

Les certificats de serveur de messagerie acceptent le domaine par défaut, internetofthings.ibmcloud.com.
Le format suivant doit être respecté pour le champ CN ou SubjectAltName du certificat :

- `orgId.messaging.internetofthings.ibmcloud.com` (domaine IoTP)

L'exemple suivant montre un CN valide pour le certificat de serveur :

`mtxpd0.messaging.internetofthings.ibmcloud.com`

Pour plus d'informations sur les certificats de serveur de messagerie, consultez la recette [ Connect Raspberry Pi to IBM Watson IoT Platform using Self-Signed Server Certificate ![External link icon](../../../../icons/launch-glyph.svg) "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}

### Domaines personnalisés (bêta)
{: #custom_domains}

**Important** : La fonction Domaines personnalisés pour les certificats de serveur de messagerie
n'est disponible que dans le cadre d'un programme bêta limité.
Pour en bénéficier,
activez les **Fonctions expérimentales** sur la page **Paramètres**. 

Avec la fonction Bêta, les certificats de serveur de messagerie acceptent les domaines personnalisés. Le format suivant doit être respecté pour le champ CN ou SubjectAltName du certificat :

- `orgId.messaging<domaine personnalisé>`

Le champ **CN** accepte les caractères génériques pour les domaines personnalisés, comme illustré dans l'exemple suivant :

- `CN=*.messaging.fab-iot.com`

**Important** : Un service DNS externe est nécessaire à la résolution du domaine personnalisé par rapport au serveur de messagerie {{site.data.keyword.iot_full}}. Ce service DNS n'est pas fourni par la plateforme.

## Enregistrement de certificats d'autorité de certification pour l'authentification de terminaux
{: #reg_ca_cert}

1. Connectez-vous à {{site.data.keyword.iot_short_notm}} et accédez à **Paramètres généraux**.
2. Dans la section **Sécurité**, sous **Certificats d'autorité de certification**, cliquez sur **Ajouter un certificat**.
3. Parcourez pour sélectionner un fichier de certificat à télécharger ou faites glisser un fichier dans la fenêtre **Ajout de certificat**. Le fichier ne peut contenir qu'un seul certificat et les dates du certificat doivent être valides. Seuls les certificats au format .pem ou . der sont acceptés. Vous pouvez prévisualiser les informations de certificat dans le fichier sélectionné.
4. Entrez une description du fichier certificat.
5. Confirmez que le fichier correct est sélectionné et cliquez sur **Sauvegarder**. Le certificat sélectionné est répertorié dans le tableau et est actif par défaut.

## Enregistrement des certificats de serveur de messagerie
{: #reg_msg_cert}

Un certificat de serveur par défaut est fourni avec la plateforme. Vous pouvez utiliser le certificat par défaut ou en télécharger un de votre organisation. Si vous n'avez pas encore de certificat à utiliser, vous pouvez créer une demande pour un nouveau certificat. Après avoir reçu le nouveau certificat, vous devez le signer et le retransférer vers la plateforme.

**Remarque :** Les pages de tableau de bord de la plateforme peuvent établir des connexions internes avec le serveur de messagerie pour extraire des informations sur les terminaux. 
Lors de la configuration de certificats de serveur autosignés pour une organisation, les
utilisateurs de tableau de bord doivent ajouter le certificat de serveur en tant que
certificat de confiance dans leurs navigateurs pour éviter tout problème de
connexion, car, par défaut, les navigateurs ne reconnaissent pas le serveur de messagerie
comme serveur sûr.

## Téléchargement d'un certificat de serveur de messagerie à partir de votre organisation
{: #upload_cert}
1. Dans la section **Sécurité** de **Paramètres généraux**, sous **Certificats de serveur de messagerie**, cliquez sur **Ajouter un certificat**.
2. Parcourez pour sélectionner un fichier de certificat à télécharger ou faites glisser un fichier dans la fenêtre **Ajout de certificat**. 
3. Parcourez pour sélectionner le fichier de clé privée à télécharger ou faites glisser un fichier dans la fenêtre **Ajout de certificat**.
4. Entrez la phrase de passe de la clé privée si celle-ci a été chiffrée avec une phrase de passe.
5. Confirmez que le fichier correct est sélectionné et cliquez sur **Sauvegarder**.

## Activation d'un certificat de serveur de messagerie

Pour activer le certificat par défaut ou un autre certificat qui a déjà été
téléchargé, sélectionnez-le dans la liste
des **certificats par défaut**,
dans le tableau situé sous **Certificats de serveur de messagerie**.
Dans ce tableau, le certificat sélectionné est listé comme certificat actif.

## Demande d'un nouveau certificat de serveur de messagerie
{: #request_cert}

Si vous souhaitez utiliser un nouveau certificat de serveur de messagerie, vous pouvez générer une demande de signature de certificat pour demander un nouveau certificat.

1. Dans la section **Sécurité** de **Paramètres généraux**, sous **Certificats de serveur de messagerie**, cliquez sur **Générer la demande de signature de certificat**.
2. Entrez les détails de la demande de signature de certificat pour votre serveur, puis cliquez sur **Générer**. La demande de signature de certificat est affichée dans le tableau.
3. Téléchargez la demande et soumettez-la à une autorité de certification pour signature.
4. Après avoir obtenu un certificat, revenez à l'entrée de demande de signature de certificat dans le tableau et téléchargez le nouveau certificat. Une fois le certificat téléchargé, la demande de signature de certificat dans le tableau est remplacée par le certificat téléchargé.
