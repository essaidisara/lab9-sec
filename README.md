# Android Attack Surface Assessment with Drozer

## Présentation

Ce projet documente un audit défensif de sécurité mobile réalisé sur l'application Android **DIVA (Damn Insecure and Vulnerable App)** à l'aide de **Drozer**.

L'objectif est d'identifier les composants Android exposés, d'évaluer leur niveau de protection et de documenter les risques associés dans un environnement contrôlé.

---

## Contexte

L'analyse a été réalisée dans le cadre d'un laboratoire pédagogique de sécurité mobile.

### Périmètre

* Émulateur Android API 30
* Drozer Agent
* Application DIVA
* Android Debug Bridge (ADB)

### Hors périmètre

* Applications tierces
* Données réelles
* Systèmes de production
* Exploitation offensive

---

## Environnement

### Poste d'analyse

| Élément          | Version |
| ---------------- | ------- |
| Windows          | 11      |
| Python           | 3.14    |
| Drozer           | 3.1.0   |
| ADB              | 1.0.41  |
| Android Emulator | API 30  |

### Application auditée

| Élément | Valeur            |
| ------- | ----------------- |
| Nom     | DIVA              |
| Package | jakhar.aseem.diva |
| Version | 1.0               |

---

## Déroulement du laboratoire

### 1. Vérification ADB

```bash
adb version
adb devices
```

### 2. Installation des applications

```bash
adb install DivaApplication.apk
adb install drozer-agent.apk
```

### 3. Configuration Drozer

```bash
adb forward tcp:31415 tcp:31415
drozer console connect
```

### 4. Cartographie des composants

```text
run app.package.info -a jakhar.aseem.diva
run app.activity.info -a jakhar.aseem.diva
run app.service.info -a jakhar.aseem.diva
run app.broadcast.info -a jakhar.aseem.diva
run app.provider.info -a jakhar.aseem.diva
```

### 5. Analyse des protections

```text
run app.package.manifest jakhar.aseem.diva
run scanner.provider.finduris -a jakhar.aseem.diva
```

---

## Résultats principaux

### Activities identifiées

| Activity          | Permission |
| ----------------- | ---------- |
| MainActivity      | Aucune     |
| APICredsActivity  | Aucune     |
| APICreds2Activity | Aucune     |

### Services

Aucun service exporté détecté.

### Broadcast Receivers

Aucun receiver exporté détecté.

### Content Provider

| Élément             | Valeur        |
| ------------------- | ------------- |
| Provider            | NotesProvider |
| Exporté             | Oui           |
| Permission lecture  | Aucune        |
| Permission écriture | Aucune        |

URI accessibles :

```text
content://jakhar.aseem.diva.provider.notesprovider/notes

content://jakhar.aseem.diva.provider.notesprovider/notes/
```

---

## Vulnérabilités identifiées

### V1 - Activities accessibles sans protection

Impact :

* Accès direct à des fonctionnalités internes
* Contrôle d'accès insuffisant

---

### V2 - Content Provider exposé

Impact :

* Accès non autorisé aux données
* Exposition d'informations sensibles

---

### V3 - Application débogable

Configuration observée :

```xml
android:debuggable="true"
```

Impact :

* Augmentation de la surface d'attaque

---

### V4 - Sauvegarde activée

Configuration observée :

```xml
android:allowBackup="true"
```

Impact :

* Exposition potentielle des données applicatives

---

## preuves

<img width="310" height="56" alt="10" src="https://github.com/user-attachments/assets/f5efde20-4874-4279-b0e3-739b9345a3f3" />

<img width="476" height="422" alt="11" src="https://github.com/user-attachments/assets/bee3a9d8-66fb-40c8-b8d7-80d8ee232c2f" />

<img width="482" height="109" alt="11-1" src="https://github.com/user-attachments/assets/2da4e6b5-3919-4e87-8b55-5c58c97aea27" />



<img width="479" height="130" alt="12" src="https://github.com/user-attachments/assets/154737db-8612-4fc8-b9ed-41d966ef5a23" />

<img width="251" height="445" alt="13" src="https://github.com/user-attachments/assets/11f4501a-a0c6-4377-a377-e6204f8fe923" />

<img width="475" height="555" alt="13-1" src="https://github.com/user-attachments/assets/2d0aa96f-989c-4bca-ab71-03b5dc6121aa" />

<img width="479" height="386" alt="14" src="https://github.com/user-attachments/assets/86bef46c-dad7-4f66-94ac-7f7ab7b48c69" />

<img width="480" height="445" alt="15" src="https://github.com/user-attachments/assets/cceedc0e-8d84-4f14-a5ca-49ae064cfafc" />


<img width="482" height="260" alt="16" src="https://github.com/user-attachments/assets/a45a7fab-8c34-4ba7-a303-060d0279f318" />

<img width="482" height="232" alt="17" src="https://github.com/user-attachments/assets/77f0d263-a67a-42e5-9051-556b98a58c32" />


<img width="478" height="503" alt="18" src="https://github.com/user-attachments/assets/ac419f83-0a65-4a52-aa4d-e7730e28a8f0" />

<img width="479" height="272" alt="19" src="https://github.com/user-attachments/assets/d7de8d0a-1efb-4ddc-8abf-ce902e144e08" />

<img width="479" height="207" alt="19-service" src="https://github.com/user-attachments/assets/b7ec97ba-20e7-4489-b038-898591235b87" />

<img width="480" height="494" alt="run app package manifest jakhar aseem diva" src="https://github.com/user-attachments/assets/1962b924-9fe4-4ec8-a92f-af02d2a4590b" />



## Références

* OWASP MASVS
* OWASP MASTG
* Drozer Framework
* Android Security Best Practices

---

## Auteur

**sara Essaidi**

Audit réalisé dans un environnement pédagogique et autorisé.
