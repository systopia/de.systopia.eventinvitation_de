# CiviCRM Event-Einladungen

## Umfang

Diese Erweiterung ermöglicht es Ihnen, Kontakte in CiviCRM zu einem Event
einzuladen und bietet ein einfaches Feedback-Formular, in dem die Kontakte
wählen können, ob sie teilnehmen können oder nicht. Sie können Kontakte per
E-Mail oder Brief einladen die einen personalisierten Link enthalten der auch
als QR-Code ausgegeben werden kann. Derzeit bietet die Erweiterung nur ein sehr
simples integriertes Feedback-Formular. Dieses Formular verfügt derzeit nur über
eine einfache "Anmelden"-Schaltfläche, die den Status der Anmeldung auf
*Bestätigt* setzt. Wir empfehlen Ihnen daher, eine externe Zielseite / einen
Endpunkt für das Formular zu erstellen (siehe unten). Für Drupal empfehlen wir
das [CiviRemote-Drupal-Modul](https://github.com/systopia/civiremote), welches
über viele vorgefertigte Funktionen verfügt.

Diese Erweiterung ist lizenziert
unter [AGPL-3.0](https://www.gnu.org/licenses/agpl-3.0).

## Funktionen

* Kontakten aus einer CiviCRM-Suche heraus und eine Veranstaltungseinladung per
  E-Mail oder Brief schicken.
* Nutzung eines einfachen CiviCRM-nativem oder einem individuellen
  Feedback-Formular, das es den Kontakten ermöglicht, auf die Einladung zu
  antworten.
* Die Nutzung des Feedbackformular aktualisiert den Status der Anmeldung

## Anforderungen

* PHP v7.0+
* CiviCRM 5.3 Empfohlen:
* Ein System, das Ihr Feedbackformular bereitstellt, z.B. eine externe Website
  oder das CMS, auf dem CiviCRM installiert ist.
  *Bemerkung*: Diese Erweiterung
  verwendet [Chillerlans QR-Code-Generator](https://github.com/chillerlan/php-qrcode),
  um QR-Codes zu erzeugen.

## Konfiguration

Navigieren Sie zu den Einstellungen der Erweiterung unter → Verwaltung →
Verwaltungskonsole → Eventeinladungskonfiguration
(`/civicrm/eventinvitation/settings?reset=1`) und geben Sie Informationen über
den Endpunkt an, wenn Sie einen verwenden möchten. Erstellen Sie mindestens eine
Nachrichtenvorlage, die eine der folgenden Smarty-Variablen enthält:

* `{$qr_event_invite_code}` - erzeugt einen eindeutigen Link für den Teilnehmer
* `{$qr_event_invite_code_img}` - generiert einen eindeutigen Link für den
  Teilnehmer, der als QR-Code mit fester Größe dargestellt wird
* `{$qr_event_invite_code_data}` - generiert einen eindeutigen Link für den
  Teilnehmer, der als QR-Code dargestellt und als Bild HTML-formatiert werden
  kann

### Verwendung eines Drupal-Endpunkts

Wenn Sie einen Drupal-Endpunkt verwenden, der auf CiviRemote basiert, besuchen
Sie die Einstellungsseite der Erweiterung, kreuzen Sie das Feld für die
benutzerdefinierte URL an und geben Sie als benutzerdefinierte URL eine der
folgenden ein, abhängig von Ihrer Konfiguration und ggf. eigenen
Implementierungen:
* `https://yourpublicfrontend.org/civiremote/event/register/{token}`
  Mit dem *register*-Endpunkt wird die Benutzerreaktion auf die Einladung als
  Registrierung verarbeitet, d.h. der Benutzer muss die Berechtigung zur
  Registrierung besitzen, wofür u.a. die Registrierung für die Veranstaltung
  noch geöffnet sein muss, und keine anderen Beschränkungen aktiv sein dürfen.
* `https://yourpublicfrontend.org/civiremote/event/update/{token}`
  Mit dem *update*-Endpunkt wird die Benutzerreaktion auf die Einladung als
  Aktualisierung einer Registrierung verarbeitet (die technisch betrachtet
  bereits mit dem Status *Eingeladen* existiert), d.h. der Benutzer muss die
  Berechtigung zum Bearbeiten eigener Registrierungen besitzen.

Installieren Sie auf Ihrem öffentlichen Drupal-System
[CiviRemote](https://github.com/systopia/civiremote), fügen Sie ein CiviRemote-
Profil (`/admin/config/cmrf/profiles`) und einen Connector
(`/admin/config/cmrf/connectors`) hinzu, falls Sie dies noch nicht getan haben.

## Verwendung

Wählen Sie aus einem Kontakt-Suchergebnis eine beliebige Anzahl von Kontakten
aus und wählen Sie im Aktionsmenü die Option "Zu Veranstaltung einladen". Es
erscheint ein Popup-Dialog, in dem Sie auswählen können:

* die Veranstaltung, zu der Sie die Kontakte einladen möchten
* die zu verwendende Nachrichtenvorlage
* die Absenderadresse
* die Rolle, die den Teilnehmer*innen zugewiesen werden soll
* ob Sie E-Mails versenden oder PDF-Dateien generieren möchten Die Erweiterung
  erlaubt es nur, Kontakte einzuladen, die nicht bereits eine Registrierung mit
  einer positiven Klasse für die betreffende Veranstaltung haben. Sie müssen
  eine Vorlage verwenden, die mindestens eines der oben beschriebenen Token
  enthält. Nachdem Sie auf "Aktion bestätigen" geklickt haben, wird für alle
  ausgewählten Kontakte ein Teilnehmerobjekt (Anmeldung) mit dem Status "
  Eingeladen" erstellt. Wenn die Kontakte den Feedback-Link verwenden, wird
  diese Anmeldung auf den Status "Eingeladen" oder "Abgesagt" aktualisiert, je
  nachdem, was der Benutzer ausgewählt hat. Wenn Sie einen automatisierten
  E-Mail-Workflow, das Einchecken von Teilnehmern über QR-Codes und/oder
  benutzerdefinierte Fernregistrierungsformulare nutzen möchten, sollten Sie
  sich die folgenden Erweiterungen ansehen:

* [Remote Events - Erstellen Sie individuelle Registrierungsformulare und Workflows für Veranstaltungen](https://github.com/systopia/de.systopia.remoteevent)
* [Event Checkin - verwenden Sie QR-Codes (z.B. auf Veranstaltungstickets), um Teilnehmer in CiviCRM Events einzuchecken](https://github.com/systopia/eventcheckin)
* [Event-Kommunikation - Definieren Sie Regeln für den Versand von Event-Mails basierend auf der Rolle, dem Status usw. der Teilnehmer (inkl. Anhänge)](https://github.com/systopia/eventmessages)

## Bekannte Probleme

Das eingebaute Feedback-Formular ist immer noch sehr simpel und in seiner
Funktionalität begrenzt. Wenn Sie das eingebaute Formular nicht selbst erweitern
wollen oder bereit sind, eine Entwicklung zu finanzieren, um die Funktionen zu
erweitern, würden wir Ihnen empfehlen, eine externe Zielseite / einen Endpunkt
für das Formular zu erstellen. Für Drupal werden Sie höchstwahrscheinlich
das [CiviRemote-Drupal-Modul](https://github.com/systopia/civiremote) verwenden
wollen, das eine Menge vorgefertigter Funktionen enthält.

## Documentation
- EN: https://docs.civicrm.org/eventinvitation/en/latest
- DE: https://docs.civicrm.org/eventinvitation/de/latest