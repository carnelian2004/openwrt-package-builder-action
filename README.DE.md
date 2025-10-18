# OpenWrt Package Builder

<p align="center">
  <a href="https://github.com/features/actions">
    <img src="https://img.shields.io/badge/Powered%20by-GitHub%20Actions-blue?logo=github-actions" alt="Powered by GitHub Actions">
  </a>
  <a href="https://github.com/Tokisaki-Galaxy/openwrt-package-builder-action/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  </a>
  <a href="https://github.com/Tokisaki-Galaxy/openwrt-package-builder-action/issues">
    <img src="https://img.shields.io/github/issues/Tokisaki-Galaxy/openwrt-package-builder-action" alt="GitHub issues">
  </a>
   <a href="https://github.com/Tokisaki-Galaxy/openwrt-package-builder-action/stargazers">
    <img src="https://img.shields.io/github/stars/Tokisaki-Galaxy/openwrt-package-builder-action" alt="GitHub stars">
  </a>
</p>

<p align="center">
  <a href="README.CN.md"><img src="https://img.shields.io/badge/简体中文-brightgreen.svg" alt="简体中文"></a>
  <a href="README.md"><img src="https://img.shields.io/badge/English-blue.svg" alt="English"></a>
  <a href="README.DE.md"><img src="https://img.shields.io/badge/Deutsch-orange.svg" alt="Deutsch"></a>
</p>

Ein einfacher, vielseitiger und sofort einsatzbereiter GitHub Actions Workflow zur automatischen Kompilierung von OpenWrt-Paketen. Dieses Projekt bietet zwei Kompilierungsmodi, um unterschiedlichen Anforderungen gerecht zu werden: **SDK-Modus (Empfohlen)** und **Quellcode-Modus**.

Sie müssen lediglich den Quellcode Ihres Pakets im Repository platzieren, eine Workflow-Datei auswählen, zwei Variablen ändern, und die `.ipk`-Dateien werden automatisch für mehrere Zielarchitekturen erstellt.

## ✨ Funktionen

-   **🚀 Einfach zu bedienen**: Kopieren Sie einfach die Workflow-Datei und passen Sie zwei Umgebungsvariablen an, um loszulegen.
-   **⚡️ Extrem schnelle Builds (SDK-Modus)**: Nutzt offizielle, vorkompilierte SDKs, um die Build-Zeit von über 30 Minuten auf unter 5 Minuten zu reduzieren.
-   **💡 Flexibel & Leistungsstark (Quellcode-Modus)**: Kann jedes Paket kompilieren, einschließlich Kernel-Module, und unterstützt Entwicklungszweige wie `master`.
-   **⚙️ Sehr vielseitig**: Kann zur Kompilierung aller Arten von OpenWrt-Paketen verwendet werden, wie z.B. LuCI-Anwendungen, Kommandozeilen-Tools oder Kernel-Module.
-   **🎯 Multi-Target-Kompilierung**: Vorkonfiguriert für gängige Architekturen wie `x86_64`, `armv8` und `mips_mt7621` und leicht anpassbar.
-   **📦 Automatische Artefakte**: Nach erfolgreicher Kompilierung werden die `.ipk`-Dateien automatisch als Build-Artefakte hochgeladen und können einfach heruntergeladen werden.
-   **💬 Kommentare zu Commits**: (Optional) Nachdem alle Build-Jobs erfolgreich waren, wird automatisch ein Kommentar zum entsprechenden Commit mit einem Link zu den Artefakten gepostet.

## 🤔 Welchen Kompilierungsmodus soll ich wählen?

Dieses Projekt bietet zwei Workflows. Bitte wählen Sie einen je nach Ihren Anforderungen.

| Merkmal / Szenario | ✅ SDK-Modus (Empfohlen) | ⚠️ Quellcode-Modus |
| :--- | :--- | :--- |
| **Build-Geschwindigkeit** | **Extrem schnell** (i.d.R. < 10 Min.) | Langsam (erster Build > 40 Min., danach schneller mit Cache) |
| **Geeignete Pakettypen** | Die meisten **LuCI-Apps** und **normale Pakete** | **Alle Pakete**, insbesondere **Kernel-Module (`kmod-xxx`)** |
| **Unterstützte OpenWrt-Versionen** | Nur **offizielle stabile Versionen** (z.B. `v23.05.3`) | **Alle Versionen**, einschließlich des `master`-Entwicklungszweigs |
| **Workflow-Datei** | `build-with-sdk.yml` | `build-from-source.yml` |

**Kurz gesagt: Wenn Sie unsicher sind oder nur eine LuCI-Anwendung kompilieren, wählen Sie den `SDK-Modus`.**

## 🚀 Anwendung

#### Beispiel

Falls Sie eine Referenz benötigen, können Sie sich dieses [Beispiel-Repository](https://github.com/Tokisaki-Galaxy/luci-app-tailscale-community/.github/workflows/build.yml) ansehen.

#### Schritt 1: Workflow-Datei auswählen und erstellen

Erstellen Sie basierend auf Ihrer obigen Wahl das Verzeichnis `.github/workflows/` und die entsprechende YAML-Datei im Stammverzeichnis Ihres Repositorys.

*   **Für den SDK-Modus**: Erstellen Sie `.github/workflows/build.yml`
*   **Für den Quellcode-Modus**: Erstellen Sie `.github/workflows/build-nonSDK.yml`

#### Schritt 2: Workflow-Code kopieren

Kopieren Sie den vollständigen Code aus der entsprechenden Datei in diesem Repository.

*   ➡️ [**Code für `build-with-sdk.yml`**](build.yml)
*   ➡️ [**Code für `build-from-source.yml`**](build-nonSDK.yml)

#### Schritt 3: Konfiguration anpassen

Öffnen Sie die von Ihnen erstellte YAML-Datei. Sie müssen nur **zwei** Variablen im `env`-Abschnitt ändern:

1.  **`PACKAGE_NAME`**:
    Ändern Sie den Wert auf den Verzeichnisnamen Ihres Pakets. **Dies ist entscheidend** und muss exakt mit dem Ordnernamen Ihres Pakets im Repository übereinstimmen.

2.  **`OPENWRT_VERSION`**:
    Legen Sie die OpenWrt-Version fest, für die Sie kompilieren möchten.
    *   **Im SDK-Modus**: Dies **muss** ein offizieller Release-Tag sein, z.B. `v23.05.3`.
    *   **Im Quellcode-Modus**: Dies kann ein beliebiger Tag oder Branch-Name sein, z.B. `v23.05.3` oder `master`.

> **Tipp:** Vergessen Sie nicht, die Badge-Links oben in Ihrer `README.md`-Datei zu aktualisieren! Ersetzen Sie `Tokisaki-Galaxy/openwrt-package-builder-action` durch Ihren eigenen Repository-Namen.

#### Schritt 4: Makefile überprüfen (Wichtig)

Stellen Sie sicher, dass die `Makefile`-Datei Ihres Pakets alle Abhängigkeiten korrekt definiert. Dieser Workflow verlässt sich auf `make defconfig`, um diese automatisch aufzulösen und auszuwählen.

Beispiel:
```makefile
define Package/my-cool-package
  # ...
  DEPENDS:=+libcurl +libjson-c +some-other-package
endef
```

#### Schritt 5: Committen und ausführen

Committen Sie Ihren Code und die neue Workflow-YAML-Datei und pushen Sie sie nach GitHub. GitHub Actions wird automatisch gestartet.

Sie können den Build-Fortschritt im `Actions`-Tab Ihres Repositorys verfolgen. Nach Abschluss finden Sie die `Artefakte` auf der entsprechenden Seite des Laufs zum Herunterladen.

## 🔧 Anpassung

### Build-Ziele anpassen

Wenn Sie für andere Architekturen kompilieren müssen, ändern Sie den Abschnitt `jobs.build.strategy.matrix.target` in der Workflow-Datei. Fügen Sie einfach Einträge im bestehenden Format hinzu oder bearbeiten Sie sie.

> **Hinweis**: Die `matrix`-Struktur unterscheidet sich zwischen den beiden Modi geringfügig. Bitte beachten Sie das Format in der jeweiligen Datei.

### Trigger anpassen

Sie können den `on`-Abschnitt am Anfang der Datei ändern, um die Auslöser (Trigger) des Workflows anzupassen, z.B. die Ausführung bei einem `pull_request` oder nach einem Zeitplan (`schedule`).

---

## Lizenz

Dieses Projekt ist unter der [MIT-Lizenz](LICENSE) lizenziert.