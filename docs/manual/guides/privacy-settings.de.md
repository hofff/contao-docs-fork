---
title: "Datenschutz Einstellungen"
description: "Übersicht der Einstellungen hinsichtlich Datenschutz: Was wird wann wo und wie lange gespeichert!"
url: "anleitungen/datenschutz"
aliases:
    - /de/anleitungen/datenschutz/
weight: 5
tags: 
   - "Datenschutz"
   - "Speicherzeiten"
---

Beim Besuch einer Website werden verschiedene Informationen gespeichert. Das hat verschiedene Gründe:

- Sicherheit (z.B. das `csrf_contao_csrf_token`)
- Nachvollziehbarkeit (z.B. die Double-Op-In Übersicht)
- Fehlerbehebung (z.B. im Error-Log)
- verschiedene Funktionen (z.B. Login oder Formularversand)

Die Websitebesucher:innen müssen über diese Datenerhebung aufgeklärt werden. Nachfolgend ein Überblick über die Informationen, die Contao speichert.

## Contao Core

### Cookies

Der Contao Core setzt grundsätzlich erstmal **keine** Cookies (ab 4.9). Es gibt jedoch drei Ausnahmen:

1. Auf der Website wird ein Formular genutzt, dann wird auf der Seite mit dem Formular ein CSRF-Token (`csrf_contao_csrf_token`) gesetzt, um CSRF-Attacken ([Cross Site Requests Forgery](https://de.wikipedia.org/wiki/Cross-Site-Request-Forgery)) zu unterbinden.
2. Auf der Website wird ein Login verwendet, dann wird nach erfolgreichem Login ein PHP-Session-Cookie (`PHPSESSID`) gesetzt, um die Login-Informationen über mehrere Seiten der Internetpräsenz weiter zureichen.
3. Auf der Website wird ein Login mit "Autologin erlauben" bzw. "Angemeldet bleiben" verwendet, dann wird nach erfolgreichem Login ein Cookie (`REMEMBERME`) gesetzt, um die Login-Informationen über einen bestimmten Zeitraum zu speichern.

Diese Cookies sind absolut ungefährlich und beinhalten lediglich eine zufällige ID, welche aber seitens Contao nicht gespeichert wird.

#### CSRF-Token

| Eigenschaft | Beschreibung                                                 |
| ----------- | ------------------------------------------------------------ |
| Name:       | `csrf_contao_csrf_token` Der Name kann in der `config.yml` individualisiert werden – siehe [System > Einstellungen](../system/einstellungen/#config-yml) |
| Typ:        | HTTP-Cookie                                                  |
| Zweck:      | Erhöht die Sicherheit der Website gegen CSRF-Attacken (Cross Site Requests Forgery). |
| Gültigkeit: | Sitzung (je nach Browser: Browser beendet, Fenster oder Tab geschlossen) |
| Anbieter:   | Eigentümer der Website (keine Übermittlung an Drittanbieter) |

#### PHP-Session-Cookie

| Eigenschaft | Beschreibung                                                 |
| ----------- | ------------------------------------------------------------ |
| Name:       | `PHPSESSID` Der Name kann in den PHP-Einstellungen des Webspaces oder in den  individualisiert werden. Der Name kann in der `config.yml` individualisiert werden – siehe [System > Einstellungen](../system/einstellungen/#config-yml) -- Dokumentation? https://symfony.com/doc/current/components/http_foundation/session_configuration.html#configuring-php-sessions |
| Typ:        | HTTP-Cookie                                                  |
| Zweck:      | Identifiziert die aktuelle PHP-Session, um bestimmte Informationen über mehrere Seiten zur Verfügung zu stellen. |
| Gültigkeit: | Sitzung (je nach Browser: Browser beendet, Fenster oder Tab geschlossen) |
| Anbieter:   | Eigentümer der Website (keine Übermittlung an Drittanbieter) |

#### RememberMe-Cookie

| Eigenschaft | Beschreibung                                                 |
| ----------- | ------------------------------------------------------------ |
| Name:       | `REMEMBERME` Der Name kann in den PHP-Einstellungen des Webspaces oder in den  individualisiert werden. Der Name kann in der `config.yml` individualisiert werden – siehe [System > Einstellungen](../system/einstellungen/#config-yml) -- Dokumentation? https://symfony.com/doc/current/security/remember_me.html |
| Typ:        | HTTP-Cookie                                                  |
| Zweck:      | Identifiziert die aktuelle PHP-Session, um die Login-Informationen über einen bestimmten Zeitraum zu speichern. |
| Gültigkeit: | 1 Jahr                                                       |
| Anbieter:   | Eigentümer der Website (keine Übermittlung an Drittanbieter) |



### Registrierung/Newsletter/Benachrichtigung bei Kommentaren/Passwort vergessen

| Eigenschaft                                                  | Beschreibung                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Welche Daten werden gespeichert?                             | - Token<br />- Datum/Uhrzeit der Anmeldung<br />- Datum/Uhrzeit der Bestätigung<br />- E-Mail-Adresse |
| Double Opt-In?                                               | ja                                                           |
| Gültigkeitsdauer Bestätigungslink?                           | 24 h                                                         |
| Wie wird mit den Daten umgegangen, wenn der Bestätigungslink **nicht** binnen 24 h bestätigt wird? | Die Daten werden spätestens nach 3 Tagen automatisch gelöscht. |
| Wie wird mit den Daten umgegangen, wenn der Bestätigungslink binnen 24 h bestätigt wird? | Die Daten werden für 3 Jahre gespeichert.<br /><br />Wenn der bestätigte Eintrag nach 3 Jahren noch vorhanden ist, wird das Token automatisch um weitere 3 Jahre (nach Bestätigungsdatum) verlängert.<br /><br />Wenn der bestätigte Eintrag nach 3 Jahren **nicht** mehr vorhanden ist, wird das Token automatisch nach 3 Jahren (nach Bestätigungsdatum) gelöscht. |
| Benamung der Token                                           | - com-\*\*\* – Kommentarbenachrichtigungen<br />- nl-\*\*\* – Newsletteranmeldung<br />- pw-\*\*\* – Passwort vergessen<br />- reg-\*\*\* – Registrierung |



### System-Log/Logfiles

Contao speichert verschiedene Informationen unterschiedlich lange. Neben Informationen die ausschließlich für Backendbenutzer relevant sind, werden für Websitebesucher:innen folgende Informationen gespeichert:

***Sind diese Zeiten noch aktuell?***

 *   Speicherzeit für Undo-Schritte (24 Stunden = 86400 Sekunden)
 *   Speicherzeit für Versionen (90 Tage = 7776000 Sekunden)
 *   Speicherzeit für Log-Einträge (14 Tage = 1209600 Sekunden) – logPeriod = period of time log entries are kept (default: 14 days)
 *   Verfallszeit einer Session (60 Minuten = 3600 Sekunden) – sessionTimeout = period of time sessions are kept (default: 1 hour)
 *   Autologin-Zeitraum (90 Tage = 7776000 Sekunden)
 *   Wartezeit bei gesperrtem Konto (5 Minuten = 300 Sekunden) – lockPeriod = period of time an account is locked (default: 5 minutes)

***Können diese individualisiert werden?***

***Gib es weitere Zeiten?***

***Welche Informationen speichert Contao im System-Log?***

***Welche Informationen speichert Contao in den Logfiles?***

***Wann werden diese Daten gelöscht?***

- Browsertyp/Browserversion
- verwendetes Betriebssystem
- Sprache und Version der Browsersoftware
- Datum und Uhrzeit des Zugriffs
- Hostname des zugreifenden Endgeräts
- IP-Adresse
- Inhalt der Anforderung (konkrete Webseite)
- Zugriffsstatus/HTTP-Statuscode
- Websites, die über die Website aufgerufen werden
- Referrer URL (die zuvor besuchte Website)
- Meldung, ob der Aufruf erfolgreich war
- Übertragene Datenmenge
- Zeitzonendifferenz zu GMT

### Formular

Beim Versand eines Formulares erstellt Contao einen Eintrag im System-Log. Der Eintrag enthält folgende Informationen:

- Datum und Uhrzeit, wann das Formular versendet wurde
- E-Mail-Adresse des Empfängers
- Browsertyp/Browserversion
- verwendetes Betriebssystem

***Wie lange werden diese Informationen gespeichert?***

***Werden dazu noch irgendwo Informationen gespeichert (z.B. in den Logfiles)?***

***Wer kann die Einträge in den Logfiles sehen?***

### Login

### Kommentare
