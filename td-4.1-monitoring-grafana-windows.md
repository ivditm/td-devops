
# 🧪 TD-4.2 – Déployer Grafana Alloy sur Windows 11 et monitorer sa machine

# 🧭 Partie 1 – Création de l’environnement Grafana Cloud

## 🔹 Étape 1 – Création du compte

1. Aller sur :
   [https://grafana.com/products/cloud/](https://grafana.com/products/cloud/)
2. Cliquer sur **Start for free**
3. Créer un compte

---

### ❓ Questions d'un bon élève NAIL

1. Quelle est la différence entre Grafana OSS et Grafana Cloud ?
2. Pourquoi utilise-t-on un service SaaS pour ce TD plutôt qu’un Prometheus local ?

---

## 🔹 Étape 2 – Identifier les endpoints

Dans le Cloud Portal, relever :

* Hosted Prometheus URL : `____________________`
* Hosted Prometheus ID : `____________________`
* Loki URL : `____________________`
* Loki ID : `____________________`

---

## 🔹 Étape 3 – Création d’une Cloud Access Policy

Créer une policy nommée :

```
alloy-windows
```

Avec permissions :

* MetricsPublisher
* LogsPublisher

Générer la clé et la stocker temporairement.

---

### ❓ Questions d'un bon élève NAIL

5. Pourquoi ne faut-il jamais hardcoder une clé API dans un fichier de configuration ?
6. Quel serait le risque si la clé était exposée publiquement ?

---

# 🖥 Partie 2 – Installation de Grafana Alloy

## 🔹 Étape 1 – Télécharger Alloy

PowerShell (Admin) :

```powershell
Set-Location $env:TEMP

Invoke-WebRequest `
  "https://storage.googleapis.com/cloud-onboarding/alloy/scripts/install-windows.ps1" `
  -OutFile "install-windows.ps1"
```

---

## 🔹 Étape 2 – Installer

  ```powershell
  powershell -ExecutionPolicy Bypass -File ".\install-windows.ps1"
  ```

Vérifier le service :

  ```powershell
  sc.exe query Alloy
  ```

---

### ❓ Questions d'un bon élève NAIL

7. Pourquoi faut-il lancer PowerShell en administrateur ?
8. Quelle est la différence entre un processus et un service Windows ?

---

# 🌍 Partie 3 – Variables d’environnement Windows

## 🔹 Étape 1 – Définir les variables (CMD Admin)

  ```cmd
  setx GCLOUD_HOSTED_METRICS_URL "VOTRE_URL_PROMETHEUS" 
  setx GCLOUD_HOSTED_METRICS_ID "VOTRE_ID_PROMETHEUS" 
  setx GCLOUD_SCRAPE_INTERVAL "60s" 
  setx GCLOUD_HOSTED_LOGS_URL "VOTRE_URL_LOKI" 
  setx GCLOUD_HOSTED_LOGS_ID "VOTRE_ID_LOKI" 
  setx GCLOUD_RW_API_KEY "VOTRE_CLE_API" 
  ```

---

## 🔹 Étape 2 – Vérification

Nouvelle fenêtre CMD (pour pas avoir le "cache"):

  ```cmd
  echo %GCLOUD_HOSTED_METRICS_URL%
  ```
Sinon redémarrer votre PC Windows... Ou `source` la variable à check

---

### ❓ Questions d'un bon élève NAIL d'un bon élève NAIL

  * Quelle est la différence entre variable d’environnement Process et Machine ?
  * Pourquoi devons-nous redémarrer le service Alloy après modification ?

---

# ⚙️ Partie 4 – Configuration Alloy

Modifier :

```
C:\Program Files\GrafanaLabs\Alloy\config.alloy
```

Contenu minimal :

```hcl
prometheus.remote_write "metrics" {
  endpoint {
    url = sys.env("GCLOUD_HOSTED_METRICS_URL")

    basic_auth {
      username = sys.env("GCLOUD_HOSTED_METRICS_ID")
      password = sys.env("GCLOUD_RW_API_KEY")
    }
  }
}

loki.write "logs" {
  endpoint {
    url = sys.env("GCLOUD_HOSTED_LOGS_URL")

    basic_auth {
      username = sys.env("GCLOUD_HOSTED_LOGS_ID")
      password = sys.env("GCLOUD_RW_API_KEY")
    }
  }
}

prometheus.exporter.windows "windows" {}

prometheus.scrape "windows" {
  targets         = prometheus.exporter.windows.windows.targets
  scrape_interval = sys.env("GCLOUD_SCRAPE_INTERVAL")
  forward_to      = [prometheus.remote_write.metrics.receiver]
}

loki.source.windowsevent "system_logs" {
  eventlog_name = "System"
  forward_to    = [loki.write.logs.receiver]
}
```

---

Redémarrer Alloy :

```powershell
sc.exe stop Alloy
sc.exe start Alloy
```

---

### ❓ Questions d'un bon élève NAIL

11. À quoi sert `sys.env()` ?
12. Que signifie `forward_to` dans Alloy ?
13. Pourquoi utilise-t-on `remote_write` au lieu d’un scrape direct par Grafana Cloud ?

---

# 📊 Partie 5 – Vérification

## 🔹 Test métriques

Dans Grafana → Explore → Prometheus :

```promql
up
```

Puis :

```promql
windows_system_system_up_time
```

---

## 🔹 Test logs

Explore → Loki :

```logql
{job="system_logs"}
```

---

### ❓ Questions d'un bon élève NAIL

14. Que signifie la métrique `up` ?
15. Comment savoir si Alloy envoie correctement les métriques ?
16. Pourquoi un dashboard peut afficher “No data” même si Alloy fonctionne ?

---

# 📈 Partie 6 – Import d’un Dashboard Windows

1. Dashboards → Import
2. Rechercher : `Windows Exporter`
3. Sélectionner la datasource Prometheus Cloud

---

### ❓ Questions d'un bon élève NAIL

17. Pourquoi le label `job` est-il important dans les dashboards ?
18. Quelle différence entre `instance` et `job` ?

---

# 🛠 Partie 7 – Debug avancé

Tester :

```powershell
sc.exe qc Alloy
```

Explorer les logs :

Observateur d’événements → Journaux Windows

---

## Mon point de vue de prof : 

C'est un outil de collect de log/metrics qui a les mêmes standards qu'OpenTelemetry
Lisez bien la documentation de Grafana Alloy :

[https://grafana.com/docs/grafana-cloud/send-data/alloy/introduction/](https://grafana.com/docs/grafana-cloud/send-data/alloy/introduction/)

![https://grafana.com/docs/grafana-cloud/send-data/alloy/introduction/](alloy_schema.png)