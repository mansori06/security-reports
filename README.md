# Security Assessment Report: Metasploitable 2 & PyGoat

Questo repository contiene l'analisi di sicurezza condotta su ambienti e applicazioni di test[cite: 2, 4, 11].

---

## Sintesi delle Scansioni

| Target / Ambito | Strumento | Esito | Criticità Rilevate | Azioni Immediate |
| :--- | :--- | :---: | :--- | :--- |
| **Metasploitable 2** *(Host & Network)* | Nessus, Nmap, Metasploit | **CRITICO** | 34 Vulnerabilità (7 Critical, 6 High): Backdoor FTP, RCE non autenticate[cite: 4, 6] | Isolamento logico immediato e decommissioning entro 7 giorni[cite: 7]. |
| **PyGoat** *(SAST - Codice)* | SonarQube | **CRITICO** | 524 Problematiche (23 Blocker, 146 High): Credenziali hardcoded, SQLi, XSS, RCE[cite: 9, 11, 13, 15, 16, 17] | Remediation del codice sorgente e bonifica dei segreti[cite: 15, 20]. |
| **PyGoat** *(SCA - Dipendenze)* | OSV-Scanner | **CRITICO** | 232 Vulnerabilità su `requirements.txt` (22 Critical, 94 High)[cite: 19, 22] | Upgrade urgente di Django, PyYAML, cryptography e urllib3[cite: 19, 22]. |

---

## Vulnerabilità Principali

* **Host & Infrastructure:** Esecuzione remota di codice (RCE) con privilegi di `root` su servizi obsoleti e vulnerabili[cite: 4, 6].
* **Hardcoded Secrets:** Esposizione della `SECRET_KEY` di Django e delle credenziali del database PostgreSQL all'interno del codice sorgente[cite: 11, 15].
* **Configurazioni Insicure:** Flag `DEBUG = True` attivo in ambiente applicativo[cite: 11, 16] e disabilitazione delle protezioni CSRF[cite: 11, 17].
* **Injection & Deserializzazione:** Presenza di difetti di tipo SQL Injection[cite: 11, 16], Cross-Site Scripting (XSS)[cite: 11, 16] e deserializzazione insicura (PyYAML)[cite: 19, 22].

---

##  Linee Guida di Remediation

1. **Gestione dei Segreti:** Rimuovere credenziali e API key dal codice; caricare le variabili d'ambiente via `os.environ`[cite: 15, 20].
2. **Hardening Applicativo:** Impostare `DEBUG = False`[cite: 15, 16], abilitare l'escaping HTML e utilizzare query parametrizzate/ORM nativo[cite: 15, 16].
3. **Aggiornamento Dipendenze:** Eseguire l'upgrade di Django alla versione LTS supportata e aggiornare i pacchetti di terze parti[cite: 19].
4. **CI/CD Integration:** Integrare SonarQube e OSV-Scanner nella pipeline per bloccare build con severità Blocker/High[cite: 12, 19, 20].

---

**Autore:** Charaf Mansori[cite: 2, 9]  
**Data:** Ottobre 2026[cite: 2, 9]
