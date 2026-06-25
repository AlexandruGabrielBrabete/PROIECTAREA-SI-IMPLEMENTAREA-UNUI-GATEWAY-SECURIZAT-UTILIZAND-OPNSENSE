# PROIECTAREA-SI-IMPLEMENTAREA-UNUI-GATEWAY-SECURIZAT-UTILIZAND-OPNSENSE
Proiect practic de securitate perimetrală cu OPNsense (în cadrul Google Cybersecurity Seminars). Demonstrează implementarea mecanismelor „Defense in Depth” într-un mediu virtualizat: Port Forwarding (DNAT), Rate Limiting (Anti-DoS), Port Knocking, MAC Binding și prevenirea exfiltrării datelor.

# Arhitectură și Securitate de Rețea cu OPNsense
Acest proiect a fost dezvoltat și implementat în cadrul programului **Google Cybersecurity Seminars**, având ca scop aprofundarea și validarea practică a conceptelor de securitate cibernetică la nivel de rețea. 

Acest repository pune la dispoziție un laborator complet, incluzând atât **documentația tehnică detaliată**, cât și **mașina virtuală OPNsense** exportată cu toate configurările și politicile de securitate gata aplicate.

## Informații Academice
* **Autor:** Brabete Alexandru-Gabriel
* **Instituția de Învățământ:** Universitatea Tehnică Cluj-Napoca
* **Specializarea:** Tehnologii și Sisteme de Telecomunicații
* **Anul de studiu:** Anul III 
* **Contextul Proiectului:** Google Cybersecurity Seminars

---

## Conținutul Repository-ului
Pentru a facilita reproducerea exactă a scenariilor de testare, acest repository include:
1. **Fișierele Mașinii Virtuale** Exportul fișierului **configuration.xml** al firewall-ului OPNsense pre-configurat cu toate regulile, alias-urile și serviciile activate.
2. **Documentația Detaliată (`PDF` / `DOCX`):** Raportul academic integral care cuprinde fundamentele teoretice (cu trimiteri bibliografice de specialitate), scenariile de implementare, capturi de ecran relevante și metodologia de *troubleshooting*.
3. **Scripturi auxiliare:** Scripturile utilizate pe mașinile client pentru validarea accesului.

---

## Obiectivele Proiectului
Scopul principal al acestui laborator este validarea practică a conceptelor de *Network Defense*. Proiectul simulează un mediu de tip *Enterprise* la scară redusă, creând o infrastructură rezilientă la acces neautorizat, exfiltrare de date și atacuri de tip Denial-of-Service (DoS).

## Topologie și Mediu de testare
Mediul a fost construit folosind virtualizarea de tip hypervisor (VMware), având următoarea arhitectură izolată:
* **Firewall / Gateway:** OPNsense (Gestionează rutarea, NAT-ul, serviciul DHCP și politicile de filtrare).
* **Zona WAN (Atacator / Auditor extern):** Stație Kali Linux.
* **Zona LAN (Server intern / Victimă):** Stație Ubuntu Linux.

---

## Funcționalități și Securitate Implementată

Acest proiect abordează securitatea în straturi (*Defense in Depth*), implementând următoarele mecanisme:

* **Port Forwarding Dinamic (Destination NAT):** Expunerea securizată și controlată a unui serviciu HTTP intern pe porturi non-standard, demonstrând translatarea adreselor de rețea (Layer 3) cu reguli stricte de asociere.
* **Protecție Anti-DoS și Rate Limiting:** Restricționarea conexiunilor TCP concurente per nod sursă. Sistemul include izolarea dinamică a adreselor IP abuzive, care sunt adăugate automat într-o tabelă de carantină (`virusprot`) în momentul depășirii pragurilor prestabilite.
* **Acces Condiționat prin Port Knocking:** Securizarea extremă a serviciilor de management (ex: SSH). Accesul direct este interzis implicit, porturile devenind rutabile doar pentru IP-urile externe care demonstrează cunoașterea unei secvențe de porturi pre-autorizate.
* **Securitate Layer 2 (MAC Binding / Kea DHCP):** Restricționarea alocării adreselor IP exclusiv pentru adresele MAC cunoscute și integrarea acestora cu sistemul de Alias-uri din OPNsense, garantând integritatea regulilor de filtrare chiar și în cazul modificărilor topologice.
* **Prevenirea Exfiltrării Datelor (Data Loss Prevention):** Aplicarea unei politici riguroase de tip *Default Deny* la nivelul interfeței LAN și demonstrarea blocării traficului neautorizat inițiat dinspre interior către mediul extern.

## Metodologie de validare
O componentă majoră a acestui proiect o reprezintă validarea funcționalității. Fiecare mecanism de securitate a fost testat riguros (inclusiv testarea negativă) folosind instrumente specifice:
* Utilizarea `curl` pentru testarea expunerii web și a pragurilor Rate Limiting.
* Utilizarea utilitarului `nc` (Netcat) pentru verificarea conectivității la nivelul stratului Transport.
* Analiza de tip „Deep Dive” a deciziilor firewall-ului monitorizând stările de conexiune și procesarea pachetelor în timp real prin modulul **Live View**.
