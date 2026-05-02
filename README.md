# 🛡️ Lab 7 : Analyse Dynamique avec MobSF & DIVA

Ce laboratoire détaille la mise en place d'un environnement d'analyse dynamique complet utilisant **MobSF (Mobile Security Framework)** pour auditer l'application vulnérable **DIVA** (Damn Insecure and Vulnerable Android App).

---

## 🎯 Objectifs du Laboratoire
*   Configurer un émulateur **AVD** (Android Virtual Device) optimisé pour le rooting et l'instrumentation.
*   Déployer **MobSF** via Docker pour centraliser l'analyse statique et dynamique.
*   Identifier des vulnérabilités en temps réel : fuites de logs, stockage non sécurisé et intents exposés.

---

## 🏗️ Étape 1 : Configuration de l'émulateur (AVD)
Nous utilisons une image **Google APIs (sans Play Store)** sous **API 29 ou 30** pour garantir un environnement "clean", rooté et compatible avec Frida.

> <img width="895" height="688" alt="image" src="https://github.com/user-attachments/assets/d30c6134-a41f-4e26-b3e2-63cd8c5c1e4f" />


## 🐳 Étape 2 : Lancement de MobSF via Docker
MobSF est lancé dans un conteneur Docker en le liant à l'identifiant de l'émulateur (`emulator-5554`) pour permettre l'injection de scripts Frida et l'interception réseau.

```bash
# Commande de lancement (adapter l'ID de l'émulateur)
docker run -it --rm -p 8000:8000 -e MOBSF_ANALYZER_IDENTIFIER=emulator-5554 opensecurity/mobile-security-framework-mobsf:latest
```
<img width="1248" height="370" alt="image" src="https://github.com/user-attachments/assets/7cbcdbed-8cd7-421c-88d9-93b9a4f347fa" />




<img width="1144" height="420" alt="image" src="https://github.com/user-attachments/assets/f745c8c7-bb15-4991-8770-4434253dc3e9" />


---

## 🔬 Étape 3 : Analyse Dynamique de DIVA
L'application DIVA est soumise à des tests de runtime pour observer les vulnérabilités actives.

### Points clés observés :
*   **Insecure Logging** : Capture des identifiants sensibles dans le flux Logcat.
*   **Insecure Data Storage** : Détection de fichiers XML ou bases de données écrits en clair dans `/data/data/`.
*   **Network Traffic** : Interception du trafic (HTTP/HTTPS) via le proxy global de MobSF.


> <img width="460" height="861" alt="image" src="https://github.com/user-attachments/assets/3356e7a2-3e71-42ba-a9be-d6a07b33a21a" />

<img width="1701" height="923" alt="image" src="https://github.com/user-attachments/assets/9fa8c74d-7572-4d43-8455-dd6f4dbc60f6" />

<img width="456" height="857" alt="image" src="https://github.com/user-attachments/assets/a845bf72-cbb4-4e64-871b-7d47a302e98a" />

<img width="1678" height="835" alt="image" src="https://github.com/user-attachments/assets/749fe658-650c-4402-a67a-7580ae776ebc" />



---

## 🛠️ Étape 4 : Instrumentation Frida
Nous utilisons l'onglet Frida de MobSF pour effectuer du "Hooking" de méthodes en temps réel afin de contourner certains contrôles de sécurité (ex: Root Detection ou login bypass).

<img width="859" height="652" alt="image" src="https://github.com/user-attachments/assets/2a7447e6-224b-46f2-b51b-861ef33fc087" />

<img width="508" height="543" alt="image" src="https://github.com/user-attachments/assets/3c4d83e4-385d-4eae-8ac0-72a7f755485b" />



---

## ⚠️ Dépannage (Troubleshooting)
| Problème | Solution |
| :--- | :--- |
| **Dynamic Analysis Failed** | Vérifier qu'un seul émulateur est actif et reconnu par `adb devices`. |
| **Émulateur trop lent** | Utiliser une image x86_64 avec l'accélération matérielle activée. |
| **SSL/TLS non intercepté** | S'assurer que le certificat CA de MobSF est bien installé via le bouton "Install Root CA". |

---

## 🏁 Conclusion
Ce laboratoire démontre la puissance de l'analyse hybride (statique + dynamique). En déplaçant l'audit dans un environnement runtime, nous avons pu confirmer des vulnérabilités qui restent invisibles lors d'une simple lecture de code source.

---
