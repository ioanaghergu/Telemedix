# Telemedix 🏥💻

Acest repository conține implementarea unei **Platforme de Telemedicină** care îmbunătățește accesul la îngrijire medicală prin consultații online, recomandări bazate pe inteligență artificială și extragerea simptomelor din fișiere PDF.

## Funcționalități 🚀

- **Consultații Online** 🩺💬
- **Sugestii AI pentru Specializări Medicale** 🤖💡
- **Extragerea Simptomelor din PDF** 📄🔍
- **Gestionarea Programărilor** 📅📈

## Arhitectură și Design 🏗️

Sistemul utilizează **Patternul Strategy** pentru gestionarea componentei de inteligență artificială și **Patternul Model-View-Controller (MVC)** pentru o arhitectură modulară și ușor de întreținut.

### Diagrame 📊

1. **Workflow Diagram** - Diagrama Generala:
   ![Workflow Diagram](diagrams/Diagrama%20Workflow.jpg)
   
2. **Notifications Sequence Diagram** - Melania Ion:
   ![Notifications Sequence Diagram](diagrams/Notifications%20Sequence%20Diagram.png)

## Diagrama de secvență pentru funcționalitatea - notificări
### Paricipanți
- Utilizator (actor): Inițiază acțiuni în aplicație.
- App UI: Interfața grafică ce preia acțiunile utilizatorului și comunică cu backend-ul.
- Backend: Gestionează logica aplicației.
- Database: Baza de date corespunzătoare aplicației.
  
### Notificările cu rolul de "reminder"
- Se verifică dacă utilizatorul este logat (operatorul `alt` indică cele două cazuri: **logat** sau **nu**).
- Dacă utilizatorul este logat:
  - Backend-ul caută consultații ce urmează sa aibă loc în următoarea oră.
  - Operatorul de interacțiune `opt` este utilizat pentru a reprezenta faptul că, dacă acestea există, backend-ul creează notificările cu detaliile aferente și le inserează în baza de date.
  - După acest pas, backend-ul calculează numărul notificărilor necitite (existente deja sau nou-create) și trimite această valoare către frontend pentru actualizarea clopoțelului.

### Creare consultație
- Când un pacient creează o noua consultație cu succes (operatorul `alt` evidențiază cele două rezultate posibile: **validare reușită** sau **eșuată**), backend-ul creează o notificare pentru medic pentru a-l informa de acest aspect.

### Anulare consultație
  - Notificările sunt generate pentru celălalt utilizator în funcție de cine anulează: **pacient** sau **medic**.
  - Operatorul `alt` determină cele două ramuri pentru a determina ce tip de mesaj se creează ca notificare și cui îi este transmis.

### Centrul de notificări
- La cererea utilizatorului, backend-ul returnează toate notificările (cele noi, precum si cele marcate deja ca "read"), începând cu cea mai recentă.

### Marcarea notificărilor ca "read"
- Backend-ul actualizează notificarea (`isRead=true`). Trimite apoi frontend-ului numărul actualizat de notificări necitite pentru clopoțel și informarea utilizatorului.

### Ștergerea notificării
- Se folosește operatorul `alt` pentru a verifica dacă notificarea este citită:
  - În caz afirmativ, este trimisă cererea pentru a fi ștearsă.
  - În caz contrar, se afișează un mesaj de eroare.
___________________________________________________________________________________________________________________________

4. **Activity Diagram - Appointment Management** - Melania Ion:
   ![Activity Diagram - Appointment Management](diagrams/Activity%20Diagram%20Appointments%20Management.png)

5. **Authentication Sequence Diagram** - Ioana Ghergu:
   ![Authentication Sequence Diagram](diagrams/Login%20and%20Sign%20up%20Sequence%20Diagram.png)

6. **Database Diagram** - Balc Larisa:
   ![Database Diagram](diagrams/Diagrama%20baza%20de%20date.jpg)

   ## **Structura bazei de date**

   ### **Tabele și atribute**

   1. **User**  
      Gestionează informațiile utilizatorilor (pacienți, medici, admini).  
      - `userID`, `email`, `password`, `username`, `birth_date`, `role`.  

   2. **Patient**  
      Conține date despre pacienți.  
      - `patientID` (cheie primară și externă), `insurance_no`, `emergency_contact`.  

   3. **Medic**  
      Gestionează datele despre medici.  
      - `medicID` (cheie primară și externă), `specializationID` (cheie externă), `licence_no`.  

   4. **Specialization**  
      Definește specializările medicale.  
      - `specializationID`, `specialization_name`.  

   5. **Service**  
      Gestionează serviciile asociate cu specializările.  
      - `serviceID`, `specializationID` (cheie externă), `service_name`.  

   6. **Appointment**  
      Centralizează programările.  
      - `appointmentID`, `patientID`, `medicID`, `availabilityID`, `serviceID`, `notes`.  

   7. **Availability**  
      Gestionează intervalele orare ale medicilor.  
      - `availabilityID`, `medicID`, `date`, `start_time`, `end_time`, `availability_status`.  

   8. **MedicalRecord**  
      Stochează informații medicale.  
      - `recordID`, `medicID`, `patientID`, `symptoms`, `diagnosis`, `treatment`.  

   ### **Relații între tabele**
   1. **User** → **Patient**, **Medic**: Toți pacienții și medicii sunt utilizatori.  
   2. **Medic** → **Specialization**: Medicii au o specializare.  
   3. **Service** → **Specialization**: Serviciile sunt legate de specializări.  
   4. **Appointment** → **Patient**, **Medic**, **Service**, **Availability**: Detaliile unei programări.  
   5. **Availability** → **Medic**: Intervalele orare disponibile ale medicilor.  
   6. **MedicalRecord** → **Patient**, **Medic**: Fișe medicale asociate cu pacienți și medici.  

7. **Use Case Diagram** - Balc Larisa:
   ![Use Case Diagram](diagrams/Diagrama%20UML%20UseCase.jpg)

   ## **Actori principali**  
   1. **Pacient**: Utilizator al platformei pentru gestionarea sănătății.  
   2. **Medic**: Specialist care oferă consultații și gestionează programări.  
   3. **Sistem AI**: Componentă automată pentru analiză medicală și asistență.  

   ## **Funcționalități**  

   ### **Pentru Pacient**  
   - **Profil medici**: Căutare și vizualizare informații.  
   - **Sugestii AI**: Recomandări de medic bazate pe datele pacientului.  
   - **Documente medicale**: Încărcare PDF și extracție simptome.  
   - **Dosar medical**: Vizualizare și actualizare istoric.  
   - **Cont**: Autentificare, editare profil.  
   - **Consultații**:  
      - Programare, notificări status, anulare/reprogramare.  
      - Vizualizare consultații viitoare/anterioare.  

   ### **Pentru Medic**  
   - **Consultații**:  
      - Vizualizare viitoare/anterioare, notificări.  
   - **Program**: Stabilire orar disponibilitate.  
   - **Consultații online**: Organizare întâlniri virtuale.   

   ## **Relație cu AI**  
   - Sugestii personalizate și analiză automată a documentelor pentru recomandări și simplificarea consultațiilor.  

   ---

8. **Class Diagram** - Bianca Andrei:
   ![Class Diagram](diagrams/Diagrama%20clase.jpg)

   Diagrama de clase ilustrează structura unui sistem de gestionare a consultațiilor medicale, având clasa **User**, moștenită de **Doctor** și **Patient**. User definește atribute generale (de exemplu username, email) și metode comune (login(), edit_account()), în timp ce Doctor include funcții specifice precum set_availability(). Pacienții pot crea programări prin metoda add_consultation() și își pot gestiona fișele medicale. Clasa **Appointment** stochează detalii legate de consultații, precum data și intervalul, medicul și observațiile, iar doctorii sunt asociați cu specializările și disponibilitățile lor (Availability). Modelul evidențiază clar relațiile dintre utilizatori, programări și componentele esențiale ale sistemului.

9. **Consultation State Diagram** - Bianca Andrei:
   ![Consultation State Diagram](diagrams/Diagrama%20stari%20consultatie.jpg)

   Diagrama de stări descrie ciclul de viață al unei consultații medicale în cadrul unui sistem de gestionare a programărilor. Procesul începe în starea inițială **Idle**, unde utilizatorul poate iniția diverse acțiuni. Consultația poate trece prin mai multe stări:

      - **Creating appointment** - Pacientul completează câmpurile formularului, iar sistemul validează datele. Dacă programarea este validă, se trimite o notificare de creare; în caz contrar, pacientul solicitant va primi un mesaj de eroare.
      
      - **Editing** - Utilizatorul poate edita notele asociate consultației, actualizând informațiile.
      
      - **Cancelling appointment** - Programarea poate fi anulată, moment în care se trimite o notificare de anulare.
      
      - **Sending notification** - Starea de trimitere a notificărilor are loc pentru acțiuni precum crearea sau anularea consultației, dar și pentru a le reaminti utilizatorilor de programare cu o oră înainte de consultație.
      
      - **Deleting appointment** - O programare deja anulată sau marcată ca fiind finalizată poate fi ștearsă definitiv din sistem.

10. **Package Diagram** - Andrei Horceag:
   ![Package Diagram](diagrams/Package%20Diagram.jpg)

11. **Deployment Diagram** - Andrei Horceag:
   ![Deployment Diagram](diagrams/Deployment%20Diagram.jpg)

## Instalare 🛠️

### 1. Clonează **repository-ul** 📥

```bash
git clone https://github.com/ioanaghergu/TeleMedix.git
```

### 2. Instalează Dependențele 📦

```bash
cd TeleMedix
pip install -r requirements.txt
```

### 3. Instalează Dependențele pentru aplicația de consultări online 📦

Aceasta a fost realizată utilizând node.js(v20.13.1). Node poate fi descărcat și instalat de aici: https://nodejs.org/en/download/current.
După instalare, vom rula următoarele comenzi:

```bash
cd videoCall
npm install mkcert -g
mkcert create-ca
mkcert create-cert
npm init -y
Get-Content dependencies.txt | ForEach-Object { npm install --save $_ }
```
Pentru a rula aplicația vom folosi comanda:

```bash
node ./signalingServer.js
```
### 4. Configurarea Parametrilor AI 🤖

#### Unzip:

```powershell
.\setup_pull.ps1
```

#### Zip:

```powershell
.\setup_push.ps1
```

---

După acești pași, platforma este gata de utilizare! 🚀
```
