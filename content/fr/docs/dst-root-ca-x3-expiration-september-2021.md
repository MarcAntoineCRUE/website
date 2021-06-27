---
title: DST Root CA X3 Expiration (September 2021)
slug: dst-root-ca-x3-expiration-september-2021
top_graphic: 1
lastmod: 2021-27-06
untranslated: 1
menu:
  main:
    weight: 30
    parent: about
---


Le 30 septembre 2021, un changement a été introduit concernant la façon dont les anciens navigateurs et appareils font confiance aux certificats de Let's Encrypt. Dans le cas où vous avez un site internet classique, vous ne  remarquerez pas de différence - la grande majorité de vos visiteurs accepteront toujours le certificat Let's Encrypt. Dans le cas où vous utilisez une API ou devez prendre en charge des appareils IoT, vous devriez prendre en compte  ce changement.

Let's Encrypt utilise un "[certificat racine][root certificate]" appelé [ISRG Root X1]. Les navigateurs et appareils modernes font confiance au certificat Let's Encrypt installés sur votre site Web, car ils incluent ISRG Root X1 dans leur liste de certificats. Afin d'assurer que les certificats que nous émettons soient fiables sur les appareils les plus anciens, nous avons également une "signature croisée" à partir d'un ancien certificat racine : DST Root CA X3.

Lorsque nous avons commencé, cet ancien certificat (DST Root CA X3) nous a aidé à démarrer et a été accepté par presque tous les appareils immédiatement. Le nouveau certificat racine (ISRG Root X1) est maintenant lui aussi largement approuvé - mais certains appareils plus anciens ne seront jamais mis à jour, et donc n'accepteront pas les nouveaux certificats (par exemple, un iPhone 4 ou un HTC Dream). [Cliquez ici pour une liste des plates-formes qui approuvent le certificat ISRG Root X1][compatibility]

Le certificat racine DST Root CA X3 va expirer le 30 septembre 2021. Cela signifie que les anciens appareils qui ne font pas confiance au certificat ISRG Root X1 commenceront à générer des avertissements de certificats lorsqu'ils accèderont à des sites utilisant des certificats Let's Encrypt. Il y a une exception importante : les anciens appareils Android qui ne font pas confiance au certificat racine ISRG Root X1 continueront à fonctionner avec Let’s Encrypt, ce [grâce à un mécanisme de signature croisée de DST Root CA X3][cross-sign] qui s’étend au-delà de l’expiration de ce certificat racine. Cette exception ne fonctionne que pour Android.

Que deviez-vous faire ? Pour la plupart des utilisateurs, rien du tout ! Nous avons configuré nos autorités de certification afin que votre site web fasse ce qu'il faut dans la plupart des cas, favorisant une large compatibilité. Si vous fournissez une API, ou avez à prendre en charge des équipements IoT, vous devriez avoir à prendre en compte deux choses : (1) tous les clients de votre API doivent prendre reconnaître le certificat ISRG Root X1 (pas seulement DST Root CA X3), et (2) si les clients de votre API utilisent OpenSSL, [ils doivent utiliser une version 1.1.0 ou plus récente][openssl]. Sous OpenSSL 1.0.x, une bizarrerie dans la vérification des certificats signifie que même les clients qui font confiance à ISRG Root X1 échoueront lorsqu'ils seront présentés avec la chaîne de certificats compatible Android que nous recommandons par défaut. 

Si vous souhaitez davantage d'informations sur les changements en cours dans notre chaine de production, [n'hésitez pas à consulter ce fil de discussion dans notre forum communautaire][production].

Si vous avez la moindre question à propos de l'expiration de certificat à venir, [merci de poster votre message sous ce fil de discussion sur notre forum][forum].

[root certificate]: /docs/glossary/#def-root
[ISRG Root X1]: /certificates/
[cross-sign]: /2020/12/21/extending-android-compatibility.html
[openssl]: https://community.letsencrypt.org/t/openssl-client-compatibility-changes-for-let-s-encrypt-certificates/143816
[forum]: https://community.letsencrypt.org/t/help-thread-for-dst-root-ca-x3-expiration-september-2021/149190
[compatibility]: /docs/cert-compat/
[production]: https://community.letsencrypt.org/t/production-chain-changes/150739