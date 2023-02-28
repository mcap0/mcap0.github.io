---
title: Anonimo e Criptato - TheTube
date: 2023-02-28 00:00:00 -500
categories: [Computer Science, Database]
tags: [cybersecurity, messaggistica]
image_path: /assets/
pin: True
--- 

Progetto Universitario, Basi di Dati A.A. 2022-2023
Matteo Capodicasa

# Presentazione

Si vuole progettare, tramite database di tipo relazionale, una piattaforma di messaggistica dove ogni chat non è mostrata in chiaro nel database, dove ogni messaggio è criptato.

Le fasi da svolgere vanno dalla progettazione concettuale a quella logica, fino alla implementazione della basi di dati e delle operazioni previste.

Verrà fornita documentazione per le seguenti:
- 1.Progettazione concettuale:
	- 1.1 Analisi dei requisiti
	- 1.2 Operazioni sui dati
	- 1.3 Struttura e raggruppamento requisiti
	- 1.4 Schema concettuale e strategia di progetto, tramite modello E/R
	- 1.5 Dizionario dei dati 
	- 1.6 Dizionario delle relazioni 
	- 1.7 Vincoli non esprimibili e dati derivabili
	- 1.8 Note sulla Sicurezza
- 2.Progettazione Logica:
	- 2.1 Tavola dei volumi 
	- 2.2 Tavola delle frequenze 
	- 2.3 Schema Logico 
- 3.Progettazione Fisica:
    - 3.1 Scelta Database e Client
    - 3.2 Implementazione MySQL
    - 3.3 Installazione e Utilizzo
    - 3.4 Codice Sorgente di The_Tube.py

# 1. Progettazione Concettuale
## 1.1 Analisi dei requisiti

Si vuole progettare la base di dati necessaria a supportare una piattaforma di messaggistica criptata end-to-end. La base di dati è incentrata sulla sicurezza e sull'efficenza delle operazioni.

Per ogni utente (circa 1000) rappresentiamo i seguenti dati:
username, public_key, hash della private_key.

Ogni utente può creare chat con altri utenti di cui conosce l'username, che riceveranno una `richiesta di chat`.

Per ogni richiesta di chat (circa 20 per utente) verranno create chat distinte per ogni partecipante.

Per ogni chat (circa 50000) rappresentiamo i seguenti dati: un idChat (che la identifica), un header criptato, un campo header_test (che servirà per autenticare i messaggi), un campo text, che conterrà l'intera chat criptata con la public_key del proprietario della chat.

Avendo una copia criptata della chat per ogni partecipante in essa, è impossibile risalire alle persone contattate da un utente, senza possederne la private_key.

Per ogni chat, aggiungiamo un record nella tabella "Chat_User_Relations", che affianca gli attributi: username e chatID.

## 1.2 Operazioni sui dati

Le principali operazioni per il database sono:
- **O1:** Registrazione nuovo utente (30 volte al giorno);
- **O2:** Login Utente (5000 volte al giorno);
- **O3:** Nuova Chat (300 volte al giorno);
- **O4:** Modifica Chat (Nuovo Messaggio) (5000 volte al giorno);
- **O5:** Cancellazione Chat (50 volte al giorno);
- **O6:** Cancellazione Utente (10 volte a settimana);
- **O7:** Accettazione Richiesta Chat (150 volte al giorno)
- **O8:** Lettura Chat (10000 volte al giorno)

## 1.3 Struttura e raggruppamento requisiti

**Dati di carattere generale:**
Si vuole progettare la base di dati necessaria a supportare una piattaforma di messaggistica criptata. Le scelte di implementazione sono incentrate sulla sicurezza. 
La piattaforma e le sue funzioni sono basate sui bisogni dell'utente, per usare la piattaforma è necessario avere un client Python per effettuare le operazioni di encrypting, decrypting dei dati.

**Dati relativi agli utenti:**
Per ogni utente della piattaforma (circa 1000) rappresentiamo i seguenti dati: 
- username (unico, che lo identifica), 
- public_key (per la criptazione dei dati), 
- SHA512hashed_private_key (per l'autenticazione).

**Dati relativi alla registrazione nuovo utente:**
Ad ogni accesso alla piattaforma, verrà richiesto all'utente di inserire il proprio username. Se l'username non viene riconosciuto, all'utente verrà richiesto se desidera registrarsi alla piattaforma. Una volta registrato, il nuovo utente riceverà una "private_key" e una "public_key". 
All'utente viene richiesto di salvare nel suo PC una copia della private_key in formato PEM per permettere l'autenticazione delle operazioni quali nuovi messaggi, nuove chat.

**Dati relativi alle chat:**
Ogni chat salvata nel database contiene informazioni necessarie a rendere la conversazione possibile e privata.
Per ogni chat (stima 50000) avremo gli attributi 
- "idChat", 
- un campo "header" criptato con le informazioni "destinatario, chat_id destinatario", 
- un campo "header_test", che contiene una combinazione cifrata di private_key (del mittente) e di username (del destinatario), ha funzione di autenticazione e di controllo ad ogni nuovo messaggio, 
- infine un campo "text", che conterrà l'intera conversazione cifrata.

**Dati relativi ai messaggi:**
Per ogni messaggio inviato si effettua una operazione di verifica del mittente e del destinatario (header_test), successivamente viene inviato al database il nuovo campo "text" della chat del mittente e del destinatario, già cifrate, che verranno sostituite alle precedenti chat con due operazioni di scrittura. In ogni messaggio vengono aggiunti data, ora, mittente sotto forma di testo.

## 1.4 Schema concettuale e strategia di progetto, modello E/R

### Schema Scheletro
![Pasted image 20230216130115.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020230216130115.png)

La strategia di progetto scelta per la costruzione dello schema Entità-Relazione (E/R) è la strategia mista, che prevede l'utilizzo di uno schema iniziale, detto schema scheletro, al quale si effettuano dei passaggi di raffinamento per ottenere uno schema E/R finale e completo.

Lo schema E/R completo verrà costruito attraverso il raffinamento delle entità utente, chat e messaggi e delle relative associazioni.

### Raffinamento Entità Utente

L'entità utente rappresenta l'utente della piattaforma e contiene le informazioni necessarie per identificarlo e autenticarlo, ovvero username, public_key e hashed_private_key.

Inoltre, l'entità private_key, che rappresenta la chiave privata dell'utente, identificato dalla hashed_private_key di un utente, non verrà memorizzata nel database, ma sarà salvata nel PC dell'utente stesso. Infatti, la sua presenza nel database renderebbe il sistema vulnerabile ad attacchi contro l'integrità, l'autenticazione e la non repudiabilità.

La cardinalità dell'associazione tra l'entità utente e l'entità chat è "uno a molti", poiché un utente può creare molte chat, ma ogni chat ha un solo creatore.

![Pasted image 20230228114635.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020230228114635.png)

### Raffinamento Entità Chat

L'entità chat rappresenta una conversazione tra due utenti della piattaforma e contiene le informazioni necessarie per identificarla e criptarla, ovvero idChat, Header, header_test e texts.

L'attributo Header contiene il nome del destinatario della chat cifrato e dell'idChat, mentre l'attributo header_test è una combinazione cifrata di chiave privata del mittente e username del destinatario. Ha la funzione di autenticazione e controllo ad ogni nuovo messaggio.

La cardinalità dell'associazione tra l'entità chat e l'entità messaggio è "uno a molti", poiché una chat può contenere molti messaggi, ma ogni messaggio appartiene ad una sola chat.

### Entità di associazione

L'entità di associazione `Chat_User_Relations`, che possiede solo "chatid" e "username" è una tabella di associazione tra l'entità "User" e l'entità "Chat" derivata dalla relazione "partecipa". Questo tipo di entità viene utilizzata quando si ha una relazione molti-a-molti tra due entità. In questo caso, ogni utente può partecipare a molte chat e ogni chat può essere partecipata da molti utenti.

La tabella di associazione contiene solo gli identificatori delle due entità correlate, ovvero "chatid" e "username". Non ha attributi aggiuntivi, poiché non rappresenta una vera e propria entità, ma solo un modo per gestire la relazione molti-a-molti.

## Schema Finale
![Pasted image 20230228125616.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020230228125616.png)

## 1.5 Dizionario dei Dati 

| **Entità**         | **Descrizione**                                                                                                                 | **Attributi**                            | **Indentificatiori**                          |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- | --------------------------------------------- |
| User               | Componente attiva nelle chat. Può creare chat e modificare lo stato del proprio account                                         | username, public_key, hashed_private_key | username                                      |
| Chat               | Insieme di messaggi inviati da due Utenti.                                                                                      | idChat, Header, header_test, texts       | idChat                                        |
| Header             | Campo della chat contenente il nome del destinatario cifrato                                                                    |                                          |                                               |
| hashed_private_key | Chiave privata dell'utente, salvata nel pc e mai nel database. Indispensabile per autenticare le operazioni e decifrare le chat |                                          | User.hashed_private_key                       |
| header_test        | combinazione di chiave privata del mittente e username destinatario per l'autenticazione dei messaggi                           |                                          | User.hashed_private_key+username_destinatario |
| public_key         | Chiava pubblica dell'utente. Chiunque può ottenerla per cifrari i messaggi da mandare                                           |                                          |                                               |

## 1.6 Dizionario delle Relazioni

| Relazione | Descrizione | Cardinalità | Entità coinvolte |
| --- | --- | --- | --- |
| partecipa | Un utente può partecipare a molte chat e ogni chat è associata a più utenti. | molti-a-molti | User, Chat, Chat_User_Relations |
| crea | Ogni chat è creata da un solo utente. | uno-a-molti | User, Chat |
| contiene | Ogni messaggio appartiene ad una sola chat e una chat può contenere molti messaggi. | uno-a-molti | Chat, Message |
| invia | Ogni messaggio è inviato da un solo utente e può essere destinato ad un solo destinatario. | uno-a-uno | User, Message, Chat |
| contiene_header_test | Ogni chat contiene un unico header_test che autentica l'utente mittente e il destinatario, ogni header_test appartiene ad una sola chat. | uno-a-molti | Chat, Header_Test |
| contiene_hashed_pk | Ogni utente possiede una sola hashed_private_key e permette al database di autenticare le operazioni effettuate  | uno-a-uno | User, hashed_private_key |
| contiene_partecipante | Ogni chat ha esattamente un partecipante e ogni partecipante può partecipare a molte chat. | molti-a-molti | User, Chat, Chat_User_Relations |
| autenticazione | L'autenticazione di un messaggio viene effettuata tramite il confronto tra l'header_test della chat e la combinazione cifrata di chiave privata del mittente e username. | uno-a-uno | User, Chat, Message, Header_Test, Public_Key |

## 1.7 Vincoli non esprimibili e dati derivabili

Vincoli di sicurezza e dati derivabili rappresentano una parte importante dell'analisi di un sistema di chat criptate. Nel contesto del presente progetto, si è riscontrato che alcuni vincoli di sicurezza richiedono l'utilizzo di tecniche di sicurezza avanzate al di fuori del diagramma E/R.

Ad esempio, è stato individuato il vincolo fondamentale di garantire che i messaggi siano accessibili solo a utenti autorizzati. Per soddisfare tale esigenza, il sistema implementa tecniche di crittografia e autenticazione avanzate. Per l'implementazione di tali tecniche di sicurezza, è necessario richiedere l'autenticazione per le operazioni specifiche indicate nel documento. In particolare, l'autenticazione è richiesta per le operazioni **O3**, **O4**, **O5**, **O6**, **O7** e **O8**.

Inoltre, è stato riscontrato che, nonostante l'utilizzo di tecniche di sicurezza avanzate, la piattaforma di chat criptate presenta alcuni dati derivabili a partire da associazioni presenti nel database. Tra i dati derivabili identificati, vi sono la lunghezza di una conversazione, l'associazione tra mittente e destinatario e la lista di nomi utenti presenti sulla piattaforma.

L'individuazione dei vincoli di sicurezza e dei dati derivabili rappresenti un aspetto fondamentale per la realizzazione di un sistema di chat criptate affidabile e sicuro, dato che la loro corretta gestione permette di garantire la protezione dei dati degli utenti e di fornire un'esperienza d'uso di alta qualità. 

--- 
## 1.8 Note sulla sicurezza

La piattaforma deve garantire la massima sicurezza per tutti gli utenti registrati. Ogni utente avrà un `nome_utente` univoco, una `public key` e l'elenco delle `chat` a cui partecipa. Dopo la registrazione, verrà generato un paio di chiavi asimmetriche tramite il criptosistema RSA. La chiave privata verrà salvata nel computer dell'utente e sarà utilizzata per decriptare i propri messaggi e accedere alla piattaforma. Inoltre, la chiave pubblica dell'utente verrà salvata in relazione all'`username` e potrà essere utilizzata da chiunque voglia criptare un messaggio da inviare all'utente.

Per inviare un messaggio, l'utente deve effettuare il login e conoscere il nome utente del destinatario. Vengono attuati controlli di sicurezza ogni volta che un utente invia un messaggio. La data e l'ora del messaggio vengono criptate e inserite nel payload. Inoltre, ogni messaggio viene salvato una volta per ogni utente coinvolto. Se il messaggio è inviato creando una nuova chat con due persone, verranno create due chat distinte, ognuna con il proprio `username` nel campo "user_name" dell'entità chat. In ogni chat, ogni messaggio sarà criptato con la chiave pubblica dell'utente a cui appartiene la chat.

In ogni dashboard, a seconda dell'utente loggato, verranno visualizzate tutte le chat e in ogni chat tutti i messaggi, grazie all'utilizzo di una stored procedure. Per salvare il nome dei partecipanti di una chat, nel `header` della chat viene criptato il nome dei destinatari, mentre in ogni messaggio viene criptato l'`username` del rispettivo mittente. Attualmente, non è possibile aggiungere partecipanti o cancellare messaggi individuali, ma solo inviare nuovi messaggi o eliminare l'intera chat.

Per quanto riguarda la sicurezza del database, possibili SQL injection o usi non autorizzati dei dati salvati sono evitati grazie alla corretta implementazione delle misure di sicurezza. Inoltre, la funzione "erase-chat" garantisce che i rispettivi utenti della chat vedano i dati della loro conversazione completamente cancellati e irrecuperabili. La piattaforma non salva mai una chiave privata dell'utente, il che rappresenta l'unico modo per visualizzare i messaggi decriptati.

---

# 2.Progettazione Logica

La progettazione logica di un database è cruciale per garantire la sicurezza delle informazioni salvate, specialmente quando si tratta di dati sensibili come le conversazioni private tra utenti. L'obiettivo principale della progettazione logica del presnte database è di garantire la segretezza delle informazioni salvate.

In questa sezione, descriveremo la stima del volume di dati che il database dovrà gestire, la frequenza delle operazioni eseguite sul database e lo schema logico scelto per il database.

## 2.1 Stime e Tavola dei volumi

Per stimare il volume di dati che il database dovrà gestire, abbiamo identificato i seguenti concetti:

| **Concetto**                | **Tipo** | **Volume** |
| --------------------------- | -------- | ---------- |
| Utente                      | E        | 1000       |
| Chat                        | E        | 20000      |
| Crea(nuova chat)            | R        | 10000      |
| Modifica Chat (messaggio)   | R        | 50000     |
| Crea Utente (registrazione) | R        | 3000       |
| Correlazione (utente-chat)  | R        | 20000     |

---
## 2.2 Tavola delle frequenze

| Operazione | Tipo | Frequenza |
| ---------- | ---- | --------- |
| **O1**     | C | 30/day    |
| **O2**     | L    | 5000/day  |
| **O3**     | C    | 300/day   |
| **O4**     | U    | 5000/day |
| **O5**     | D    | 50/day    |
| **O6**     | D    | 10/week   |
| **O7**     | U    | 150/day   |
| **O8**     | R    | 10000/day | 

Legenda:

-   R: Operazione di lettura (Read)
-   U: Operazione di aggiornamento (Update)
-   C: Operazione di creazione (Create)
-   D: Operazione di cancellazione (Delete)

## 2.3 Schema Logico

| User               | tipo    |
| ------------------ | ------- |
| **username**       | varchar |
| public_key         | varchar |
| hashed_private_key | varchar |

| Chat        | tipo     |
| ----------- | -------- |
| **chat_id** | int      |
| Header      | tinyblob |
| header_test | varchar  |
| texts       | longblob |
 
| Chat_User_Relation              | tipo    |
| ------------------------------- | ------- |
| **chatid  *chiave esterna***    | int     |
| **username   *chiave esterna*** | varchar |

Nel caso del nostro progetto, la sicurezza dei dati degli utenti era di primaria importanza. Per garantire la segretezza di ogni chat, abbiamo utilizzato un database che garantisce l'indecrifrabilità di ogni messaggio e di ogni chat senza la chiave privata dell'utente corrispondente.

Lo sviluppo di questo progetto è stata una grande opportunità per permettermi di applicare le conoscenze di progettazione di database e di sicurezza delle informazioni in un contesto reale.

---
# 3.Progettazione Fisica

## 3.1 Scelta Database e Client
La gestione del database è affidata a MySQL, che provvederà l'autenticazione e la gestione dei dati cosiddetti "at rest".
Per ottenere l'effetto "live" delle chat bisogna considerare che i dati in uso hanno una struttura più complessa:
mentre i dati "in uso" vengono elaborati nel computer dell'utente, uno schema viene elaborato con tutte le informazioni necessarie per effettuare le operazioni annesse (compreso visualizzare le proprie chat e messaggi) al momento di invio di un messaggio o di una richiesta, i dati vengono aggiornati nel server MySQL secondo questo stesso schema e decifrati solo dal client fisico, anche in tempo reale.

Il client verrà gestito da un programma python che comunicherà con MySQL Server via mysql-connector-python, per permettere l'utilizzo multipiattaforma e la semplicità d'interazione. 
Il codice inoltre verrà rilasciato open source per permettere a chiunque di effettuare modifiche personalizzate di efficenza e stile.

---
## 3.2 Implementazione MySQL 

Nella sezione **3.3** viene spiegato in dettaglio come importare tabelle e procedure usando gli script forniti.

A secuito il codice della creazione delle Tabelle, Procedure e Funzioni.

**Creazione Tabella Utenti:**
```mysql
CREATE TABLE `User` (
  `username` varchar(45) NOT NULL,
  `pub_key` varchar(512) NOT NULL,
  `pass` varchar(512) NOT NULL,
  PRIMARY KEY (`username`),
  UNIQUE KEY `username_UNIQUE` (`username`),
  UNIQUE KEY `pub_key_UNIQUE` (`pub_key`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

**Creazione Tabella Chat:**
```mysql
CREATE TABLE `Chat` (
  `idChat` int unsigned NOT NULL AUTO_INCREMENT,
  `Header` tinyblob,
  `H_Test` varchar(512) DEFAULT NULL,
  `Text` longblob,
  PRIMARY KEY (`idChat`),
  UNIQUE KEY `idChat_UNIQUE` (`idChat`)
) ENGINE=InnoDB AUTO_INCREMENT=185 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

**Creazione Tabella Chat_User_Relations:**
```mysql
CREATE TABLE `Chat_User_Relations` (
  `username` varchar(45) NOT NULL,
  `chat_id` int unsigned NOT NULL,
  PRIMARY KEY (`username`,`chat_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

### Funzioni:

**Check_Chat:**
```mysql
USE `The_Tube`;
DROP function IF EXISTS `check_chat`;

USE `The_Tube`;
DROP function IF EXISTS `The_Tube`.`check_chat`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` FUNCTION `check_chat`(username VARCHAR(45), chatid INT) RETURNS int
    READS SQL DATA
    DETERMINISTIC
BEGIN

	DECLARE chat_id_list VARCHAR(1000);
	SELECT GROUP_CONCAT(chat_id) INTO chat_id_list
	FROM Chat_User_Relations
	WHERE username = username;
	
	IF FIND_IN_SET(chatid, chat_id_list) > 0 THEN
		RETURN 1;
	ELSE
		RETURN 0;
	END IF;
RETURN 1;
END$$

DELIMITER ;
;
```

**Check_User:**
```mysql
USE `The_Tube`;
DROP function IF EXISTS `check_user`;

USE `The_Tube`;
DROP function IF EXISTS `The_Tube`.`check_user`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` FUNCTION `check_user`( user_name VARCHAR(45) ,private_key VARCHAR(512)) RETURNS int
    READS SQL DATA
    DETERMINISTIC
BEGIN
	DECLARE u_pass VARCHAR(512);
    DECLARE secret VARCHAR(512);
	SET u_pass = SHA2(private_key,512);
    
    SELECT u.pass
    INTO secret
	FROM User u
    WHERE u.username = user_name
    AND u.pass = pass;
    
    IF secret <=> u_pass THEN
		RETURN 1;
	ELSE 
		RETURN 0;
    END IF;
    
END$$

DELIMITER ;
;
```

**Check_Header:**
```mysql
USE `The_Tube`;
DROP function IF EXISTS `check_header`;

USE `The_Tube`;
DROP function IF EXISTS `The_Tube`.`check_header`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` FUNCTION `check_header`(htest VARCHAR(512), chatid2 INT) RETURNS int
    READS SQL DATA
    DETERMINISTIC
BEGIN
    
    SELECT c.h_test
    INTO @dest_test
    FROM Chat c
    WHERE c.idChat = chatid2;
    
    if SHA2(htest, 512) <=> @dest_test THEN
		RETURN 1;
    else	
		RETURN 0;
    END IF;
    
END$$

DELIMITER ;
;
```

### Stored Procedures

**Accept_Request:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `accept_request`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`accept_request`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `accept_request`(IN username VARCHAR(45),IN chatid1 INT, IN chatid2 INT, in pk VARCHAR(512), IN htest VARCHAR(512))
BEGIN
	if check_user(username, pk) <=> 1 THEN
        if check_chat(username, chatid1) <=> 1 THEN
			update Chat
            set h_test = SHA2(htest,512)
            where idChat = chatid2;
		else
        select 1 where false;
        end if;
        
    else
		select 1 where false;
	end if;
END$$

DELIMITER ;
;
```

**Add_User:**
```mysql
DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `add_user`(IN p_username varchar(45), IN p_public_key varchar(512), IN encrypted_username LONGBLOB, IN private_key VARCHAR(512))
BEGIN

  START TRANSACTION;
  
  INSERT INTO User (username, pub_key, pass)
  VALUES (p_username, p_public_key, SHA2(private_key, 512));

  INSERT INTO Chat (text)
  VALUES (encrypted_username);
  
  SET @user_chatid = last_insert_id();
  
  INSERT INTO Chat_User_Relations (username, chat_id)
  VALUES (p_username, @user_chatid);

  COMMIT;
END$$

DELIMITER ;
;
```

**Delete_Chats:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `delete_chats`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`delete_chats`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `delete_chats`(IN user_username VARCHAR(45), IN private_key VARCHAR(512))
BEGIN
	if check_user(user_username, private_key) <=> 1 THEN
		DELETE c.*
		FROM chat c
		WHERE EXISTS( SELECT *
					  FROM Chat_User_Relations cur 
					  WHERE  c.idChat = cur.chat_id
					  AND cur.username = user_username );
                      
		DELETE cur.*
		FROM Chat_User_Relations cur
		WHERE cur.username = user_username;
		
	
    ELSE
		SELECT 1 WHERE false;
    END IF;
END$$

DELIMITER ;
;
```

**Delete_User:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `delete_user`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`delete_user`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `delete_user`(IN p_username VARCHAR(45), IN private_key VARCHAR(512))
BEGIN
  IF check_user(p_username, private_key) <=> 1 THEN
	call delete_chats(p_username, private_key);  
	DELETE FROM User WHERE username = p_username;

  ELSE
	SELECT 1 WHERE false;
  END IF;

END$$

DELIMITER ;
;
```

**Erase_Chat:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `erase_chat`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`erase_chat`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `erase_chat`(IN username VARCHAR(45), IN chatid INT, IN pk VARCHAR(512))
BEGIN
	if check_user(username, pk) <=> 1 THEN
		if check_chat(username, chatid) <=> 1 THEN
			delete from Chat where idChat = chatid;
			delete from Chat_User_Relations where chat_id = chatid;
            
		else 
			select 1 where false;
		end if;
	else
		select 1 where false;
	end if;
END$$

DELIMITER ;
;
```

**Exists_User:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `exists_user`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`exists_user`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `exists_user`(IN user_name VARCHAR(45))
BEGIN
    SELECT COUNT(*) FROM User u WHERE u.username = user_name;
END$$

DELIMITER ;
;
```

**Get_Chats:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `get_chats`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`get_chats`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_chats`(IN user_username VARCHAR(45))
BEGIN
	SELECT c.Header, c.idChat
    FROM Chat c
    JOIN Chat_User_Relations cur ON c.idChat = cur.chat_id
    WHERE cur.username = user_username;
    
END$$

DELIMITER ;
;
```

**GetKey:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `get_key`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`get_key`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_key`(IN username VARCHAR(45))
BEGIN
	SELECT pub_key
    FROM User u
    WHERE u.username = username;
    
END$$

DELIMITER ;
;
```

**Get_Texts:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `get_texts`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`get_texts`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_texts`(IN user_username VARCHAR(45))
BEGIN
	SELECT c.text
    FROM Chat c
    JOIN Chat_User_Relations cur ON c.idChat = cur.chat_id
    WHERE cur.username = user_username;
    
END$$

DELIMITER ;
;
```

**Get_Texts_byID:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `get_texts_byID`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`get_texts_byID`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_texts_byID`( IN chatid INT)
BEGIN
	SELECT c.text
    FROM Chat c
    WHERE c.idChat = chatid;
    
END$$

DELIMITER ;
;
```

**Is_Request:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `is_request`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`is_request`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `is_request`(IN chatid INT)
BEGIN
	select c.h_test
    from Chat c
    where idChat = chatid;
    
END$$

DELIMITER ;
;
```

**Login:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `login`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`login`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `login`(IN username VARCHAR(45), IN pkey VARCHAR(512))
BEGIN
	if check_user(username, pkey) <=> 1 THEN
		SELECT 1;
	ELSE
		SELECT 1 WHERE false;
	END IF;
    
END$$

DELIMITER ;
;
```

**New_Chat:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `new_chat`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`new_chat`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `new_chat`(IN username1 VARCHAR(45), IN username2 VARCHAR(45), IN encrypted1 LONGBLOB, IN encrypted2 LONGBLOB, IN header1 TINYBLOB, IN header2 TINYBLOB, IN h_test2 VARCHAR(512), IN pk VARCHAR(512))
BEGIN
	if check_user(username1,pk) <=> 1 THEN

		INSERT INTO Chat(text, Header)
		VALUES (encrypted1, header1);
		
		INSERT INTO Chat_User_Relations(username,chat_id)
		VALUES(username1, last_insert_id());
        
		INSERT INTO Chat(text, Header, H_Test)
		VALUES(encrypted2, header2, SHA2(h_test2,512));
		
		INSERT INTO Chat_User_Relations(username,chat_id)
		VALUES(username2, last_insert_id());
	
		SELECT last_insert_id();
    else	
		SELECT 1 WHERE false;
	END IF;
END$$

DELIMITER ;
;
```

**New_Text:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `new_text`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`new_text`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `new_text`(IN encrypted_text1 LONGBLOB, IN encrypted_text2 LONGBLOB, IN chatid1 INT, IN chatid2 INT, IN htest VARCHAR(512))
BEGIN
	if check_header(htest, chatid2) <=> 1 THEN
		UPDATE Chat
		SET text = encrypted_text1
		WHERE Chat.idChat = chatid1;
		
		UPDATE Chat
		SET text = encrypted_text2
		WHERE Chat.idChat = chatid2;
	ELSE
		SELECT 1 WHERE false;
	END IF;
END$$

DELIMITER ;
;
```

**Sign_Chat:**
```mysql
USE `The_Tube`;
DROP procedure IF EXISTS `sign_chats`;

USE `The_Tube`;
DROP procedure IF EXISTS `The_Tube`.`sign_chats`;
;

DELIMITER $$
USE `The_Tube`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `sign_chats`(IN username1 VARCHAR(45), IN pk VARCHAR(512), IN header1 TINYBLOB, IN header2 TINYBLOB, IN htest VARCHAR(512) ,IN chatid1 INT, IN chatid2 INT )
BEGIN
	IF check_user(username1, pk) <=> 1 THEN
		IF check_header(htest, chatid2) <=> 1 THEN
			UPDATE Chat c
            SET header = header1
            WHERE c.idChat = chatid1; 
            
            update Chat c
            SET header = header2
			WHERE c.idChat = chatid2;
            Select 1;
        ELSE
			SELECT 1 where false;
            
		END IF;
    ELSE
		SELECT 1 where false;
	END IF;
END$$

DELIMITER ;
;
```

---
## 3.3 Installazione e Utilizzo

Installazione Locale via MySQL Shell
```mysql
\connect -u root
#password:root
\sql
#Switch to sql mode
CREATE DATABASE The_Tube;
use The_Tube;

\source The_TubeTables.sql # importa le tabelle
show tables; #stampa le 3 tabelle

\source TheTubeSP.sql # importa le stored procedures e le funzion
show procedure status where db = 'The_Tube'

CREATE USER 'TheTubeAPI'@'localhost' IDENTIFIED BY 'la_tua_password'; 
GRANT EXECUTE ON *.* TO 'TheTubeAPI'@'localhost';
```

Adesso che abbiamo configurato tutto, apriamo una finestra di PowerShell o Bash per eseguire lo script python `The_Tube.py`

> Se non si possiedono i moduli mysql-connector, rsa sarà necessario installarli con pip

```pip
pip install rsa;
pip install mysql-connector-python
```

```shell
mkdir keys;
python The_Tube.py
```
---
## 3.4 Codice Sorgente di The_Tube.py

```python
import mysql.connector
import rsa
import hashlib
import datetime
import time
import sys
import os

def connect_to_database():
    # funzione che ritorna la connessione al database
    connection = mysql.connector.connect(
        host="localhost",
        user="TheTubeAPI",
        database="The_Tube",
        autocommit=True
    )
    return connection


def fetch_parameters(params):
    # concatena i parametri 
    return ','.join(['"' + str(param) + '"' for param in params])

def call_stored_procedure(connection, procedure_name, parameters):
    try:
        cursor = connection.cursor()    #prova a creare un cursore
    
    except:
        connection.close()
        connection = connect_to_database()  #altrimenti chiudi la connessione e riprova
        cursor = connection.cursor()

    parameters = fetch_parameters(parameters)
    #print(f'\ncall The_Tube.{procedure_name}({parameters});')
    cursor.execute(f'\ncall The_Tube.{procedure_name}({parameters});', multi=True)
    
    result = cursor.fetchall()  # colleziona i risultati della query
    #print(result)
    return result

def register(connection, p_username):
    # connettiti, salva la chiave privata nel computer, crea l'hash della private key, crea l'encrypted username (public key) e passali alla stored procedure 

    private_key, public_key, private_pem, public_pem = generate_key_pairs()

    sha_private_key = Sha512(private_pem)

    encrypted_username = encrypt(p_username, public_key)
    encrypted_username = to_hex_string(encrypted_username)

    #print(encrypted_username)

    result = call_stored_procedure(connection, "add_user", (str(p_username),public_pem.decode(), encrypted_username, sha_private_key))
    
    return result

def delete_user(connection, username, private_pem):
    #implementa la cancellazione delle chat di un utente

    result = call_stored_procedure(connection,"delete_user", (username,Sha512(private_pem)))

    return result


def generate_key_pairs():
    # genera un key pairs RSA. ritorna la versione file di entrambi. Salva nel computer
    public_key, private_key = rsa.newkeys(512)
    with open('keys/public_key.pem', 'wb') as p:
        p.write(public_key.save_pkcs1('PEM'))
    with open('keys/private_key.pem', 'wb') as p:
        p.write(private_key.save_pkcs1('PEM'))
    public_pem = public_key.save_pkcs1('PEM')
    private_pem = private_key.save_pkcs1('PEM')
    return private_key, public_key, private_pem, public_pem

def encrypt(message, key):
    block_size = 53 
    ciphertext = b''
    message_bytes = message.encode('utf-8')
    for i in range(0, len(message_bytes), block_size):  #cripta blocchi da 53 byte 
        block = message_bytes[i:i+block_size]
        if len(block) < block_size:
            block += b' ' * (block_size - len(block))
        ciphertext += rsa.encrypt(block, key)
    return ciphertext

def decrypt(ciphertext, key):
    if ciphertext is None:
        return ciphertext
    key_size = 512
    plaintext = b''
    ciphertext_blocks = [ciphertext[i:i+key_size//8] for i in range(0, len(ciphertext), key_size//8)] # decripta in blocchi da 53 byte

    for i, block in enumerate(ciphertext_blocks):
        try:
            block_plaintext = rsa.decrypt(block, key)
            plaintext += block_plaintext
        except:
            plaintext += bytes(block)
            if i == len(ciphertext_blocks) - 1 and len(block) < key_size // 8:
                plaintext = plaintext[:-key_size // 8 + len(block)]
    return plaintext

def load_keys():
    if not os.path.exists('keys/public_key.pem'):   #se non hai le chiavi salvate ritorna
        return
    with open('keys/public_key.pem', 'rb') as p:
        public_key = rsa.PublicKey.load_pkcs1(p.read())        # leggi le chiavi salvate
    with open('keys/private_key.pem', 'rb') as p:               
        private_key = rsa.PrivateKey.load_pkcs1(p.read())       

    public_pem = public_key.save_pkcs1('PEM')
    private_pem = private_key.save_pkcs1('PEM')
    return private_key, public_key, private_pem, public_pem

def getkey(connection, username):
    # pull della chiave pubblica di un utente dal database
    key = call_stored_procedure(connection, "get_key", (username,))     
    key = key[0][0]
    key = rsa.PublicKey.load_pkcs1(key)
    return key

def get_texts_byID(connection, chatid, p_key):
    # ottieni una chat dal chatid
    texts = call_stored_procedure(connection, "get_texts_byID", (chatid,))
    chats = []
    for text in texts:
        text = text[0]
        try:
            text.decode()
        except:
            pass

        try:
            text = from_hex_string(text.decode())
        except:
            pass

        text = decrypt(text, p_key)
        chats.append(text.strip())

    return chats


def get_texts(connection, username, p_key):
    # ottieni tutti i messaggi di un utente
    texts = call_stored_procedure(connection, "get_texts", (username,))
    chats = []
    for text in texts:
        text = text[0]
        try:
            text.decode()
        except:
            pass

        try:
            text = from_hex_string(text.decode())
        except:
            pass
        #print(text)
        text = decrypt(text, p_key)
        #print(decrypt(from_hex_string(text[1].decode()), private_key))
        chats.append(text)


    return chats


def get_requests(connection, username, pk):
    # ottieni le richieste controllando l'header
    result = call_stored_procedure(connection, "get_chats", (username,))
    chats = []
    for chat in result: 
        header = chat[0]
        chatid = chat[1]
        if header is None:
            continue
        header = from_hex_string(header.decode())
        header = decrypt(header,pk)
        chat_number = header.strip().split(b"-")[1].decode()
        if is_request(connection, int(chat_number)) == 0:
            chats.append([header,chatid])
        
    return chats

def get_chats(connection, username, p_key):
    # ottieni tutte le chat di un utente e decripta gli header per stampare i mittenti
    result = call_stored_procedure(connection, "get_chats", (username,))
    chats = []
    for chat in result:
        header = chat[0]
        chatid = chat[1]
        if header is None:
            continue
        header = from_hex_string(header.decode())
        header = decrypt(header,p_key)
        chat_number = header.strip().split(b"-")[1].decode()
        if is_request(connection, int(chat_number)) == 0:
            continue        
        chats.append([header,chatid])
        #print(decrypt(from_hex_string(header.decode()), p_key))

    return chats

def is_request(connection, chatid):
    # controlla se la chat è una richiesta
    res = call_stored_procedure(connection, "is_request", (chatid,))
    #print(res)
    if res:
        if res[0][0] is None:
            return 0
        else:
            return 1
    else:
        return 1



def print_chat_list(chat_list):
    # stampa la lista di chat e ritorna la scelta convertita coi dati del database
    for i, chat in enumerate(chat_list):
        header, chatid = chat
        header = header.strip()
        if header:
            print(f"{i+1}) {header.strip().decode()} (chat:{chatid})")

    selected = None
    while not selected:
        selection = input("Seleziona una chat (1-{}): ".format(len(chat_list)))
        if selection.isdigit():
            selection = int(selection)
            if 1 <= selection <= len(chat_list):
                selected = chat_list[selection - 1]
        else:
            break

    if selected:
        return selected[1], selected[0]
    else:
        return 0, 0

def check_username(connection, username):
    # controlla se l'user esiste
    result = call_stored_procedure(connection, "exists_user", (username,))
    if len(result) >= 1:
        return result[0][0]
    else:
        return 0

def login(connection, username, key):
    # autenticazione dell'utente
    result = call_stored_procedure(connection, "login", (username, key))
    if len(result) >= 1:
        return result[0][0]
    else:
        return 0

def Sha512(key):
    # ritorna l'hashdigest SHA512 di una stringa
    if isinstance(key,bytes):
        Hashedkey=hashlib.sha512(key).hexdigest()
    else:
        Hashedkey=hashlib.sha512(key.encode('utf-8')).hexdigest()
    #print(Hashedkey)
    return Hashedkey

def to_hex_string(ciphertext):
    # ritorna una lista di bytes del testo cifrato
    hex_list = [b for b in bytearray(ciphertext)]
    return '0x' + ''.join(f'{b:02x}' for b in hex_list)

def from_hex_string(hex_string):
    # ripristina il testo cifrato da una lista di bytes
    try:
        if hex_string.startswith('0x'):
            hex_string = hex_string[2:]

    except:
        return hex_string

    return bytes.fromhex(hex_string)

def newchat(connection, username1, username2, p_key):
# per creare una chat bisogna -> avere l'username del destinatario
# ottenere la public key del destinatario
# inserire nel header (criptato) in ordine:
# destinatario, chatid del destinatario
    date_time = datetime.datetime.now()

    key = getkey(connection, username2)
    header2 = encrypt(f"{username1}", key)
    message2 = encrypt(f"Chat initiated by {username1} on {date_time}", key)

    key = getkey(connection, username1)
    header1 = encrypt(f"{username2}", key)
    message1 = encrypt(f"Chat initiated by {username1} on {date_time}", key)

    message1 = to_hex_string(message1)
    message2 = to_hex_string(message2)
    header1 = to_hex_string(header1)
    header2 = to_hex_string(header2)

    htest_plain = f"{Sha512(p_key.save_pkcs1('PEM'))}{username2}"
    htest = Sha512(htest_plain)

    pk = Sha512(p_key.save_pkcs1('PEM'))

    result = call_stored_procedure(connection, "new_chat", (username1, username2, message1, message2, header1, header2, htest ,pk))
    #print(result)
    return sign_chat(connection, username1, username2, result[0][0], htest, pk)

def sign_chat(connection, username1, username2, chatid, htest,pk):
    # salva il chat id delle chat negli header criptati dei partecipanti
    chatid1 = chatid - 1
    chatid2 = chatid
    key = getkey(connection, username1)
    new_header1 = encrypt(f"{username2}-{chatid2}", key)
    new_header1 = to_hex_string(new_header1)
    key = getkey(connection, username2)
    new_header2 = encrypt(f"{username1}-{chatid1}", key)
    new_header2 = to_hex_string(new_header2)
    
    result = call_stored_procedure(connection, "sign_chats", (username1, pk,  new_header1, new_header2, htest, chatid1, chatid2))
    

    return result

def live_chat(connection, chatid1, chatid2, pk, username2, username1):
# mostra la conversazione e manda messaggi al contempo
    while True:
        chat = get_texts_byID(connection, chatid1, pk)
        for text in chat:
            print(text.decode().strip())
        message = input("> ")
        if message == "exit":   # esci dal ciclo se l'user inserisce exit
            break
        elif message:
            message = f"[{datetime.datetime.now()}]{username1}: {message}"
            #print(message)
            
            htest_plain = f"{Sha512(pk.save_pkcs1('PEM'))}{username2}"
            print(f'{username2}')
            htest = Sha512(htest_plain)
            new_text(connection, username1, username2, chatid1, chatid2,htest, message, pk)
        else:
            continue


def new_text(connection, username1, username2, chatid1, chatid2, htest, message, pk):
    # autentica e encripta ogni nuovo messaggio 
    chat = get_texts_byID(connection, chatid1, pk)
    chat = chat[0].decode().strip()
    if not chat.endswith("\n"):
        chat += "\n" + message
    else:
        chat += message
    
    #print(chat)
    key = getkey(connection, username1)
    message1 = encrypt(chat, key)
    message1 = to_hex_string(message1)

    key = getkey(connection, username2)
    message2 = encrypt(chat, key)
    message2 = to_hex_string(message2) 


    res = call_stored_procedure(connection, "new_text", (message1, message2, chatid1, chatid2, htest))
    return res

def delete_chat(connection, username, p_key, chatid):
    print(f"Deleting chat: {chatid}...")
    call_stored_procedure(connection, "erase_chat", (username, chatid, p_key))

def accept_request(connection,user,username,chatid1, chatid2, pk):
    # inserisce l'htest nella chat del creatore della chat e permette l'invio dei messaggi
    print(f"Accepting chat: {chatid1}...")
    htest_plain = pk + user
    #print(htest_plain)
    htest = Sha512(htest_plain)
    call_stored_procedure(connection, "accept_request", (username, chatid1, chatid2, pk, htest))

def banner():
    banner = ("""████████╗██╗  ██╗███████╗    ████████╗██╗   ██╗██████╗ ███████╗
╚══██╔══╝██║  ██║██╔════╝    ╚══██╔══╝██║   ██║██╔══██╗██╔════╝
   ██║   ███████║█████╗         ██║   ██║   ██║██████╔╝█████╗  
   ██║   ██╔══██║██╔══╝         ██║   ██║   ██║██╔══██╗██╔══╝  
   ██║   ██║  ██║███████╗       ██║   ╚██████╔╝██████╔╝███████╗
   ╚═╝   ╚═╝  ╚═╝╚══════╝       ╚═╝    ╚═════╝ ╚═════╝ ╚══════╝""")
    print(banner)

def main():
    # Esegui il login richiedendo un nome utente
    banner()
    username = input("Insert Your Username: ")

    try:
        connection = connect_to_database()
        if(connection):
            print("Successfully Connected")
        else:
            print("Connection failed")
            exit()
    except mysql.connector.Error as err:
        print(f"Error: {err}")
        exit()

    if load_keys():
        private_key, public_key, private_pem, public_pem = load_keys()
    
    try:
        check_result = check_username(connection, username)
        if check_result:
            login_result = login(connection, username, Sha512(private_pem))
            if login_result:
                print(f"Welcome {username}!")
            else:
                print("Sorry! Private_Key not found...")
                exit()



        else:
            choice = input(f'Account for "{username}" does not exist yet.\nYou want to register with that username? (Y/N)\n')
            if choice == "Y" or choice == "y":
                register(connection, username)
                private_key, public_key, private_pem, public_pem = load_keys()
            else:
                print("Login failed")
                
            
    except mysql.connector.Error as err:
        print(f"Error: {err}")
        

    while True:
        try:
            check = login(connection,username,Sha512(private_pem))
        except:
            break

        if(check):
            choice = input("\n0) Exit\n1) Conversations\n2) Incoming Requests\n3) New Chat Request\n4) Delete Chat\n5) Delete Account\n")

        if choice and choice == '0':
            break

        if choice == '1':
            chats = get_chats(connection, username, private_key)
            chatid1, user = print_chat_list(chats)
            if user == 0:
                continue
            user = user.decode().strip().split("-")
            if len(user) > 1:
                live_chat(connection, chatid1, user[1], private_key, user[0], username)
            else:
                print("There was a problem with the chat. Please create a new one.")

        if choice == '2':
            requests = get_requests(connection, username, private_key)
            chatid1, user = print_chat_list(requests)
            if user == 0:
                continue
            user = user.decode().strip().split("-")
            if len(user) > 1:
                request = input("Insert '1' to accept and '2' to decline the chat request.")
                if request == '1':

                    accept_request(connection, user[0], username, chatid1, user[1], Sha512(private_pem))
                if request == '2':
                    delete_chat(connection, username, Sha512(private_pem), chatid1)
                else:
                    continue

        if choice == '3':
            user = input("Insert the recipient's Username: ")
            check = check_username(connection,user)
            if check == 0:
                print("User not Found.")
                continue
            ret = newchat(connection, username, user, private_key)
            #print(ret)
            chats = get_chats(connection, username, private_key)
            chatid1, user = print_chat_list(chats)
            user = user.decode().strip().split("-")
            if len(user) > 1:
                live_chat(connection, chatid1, user[1], private_key, user[0], username)
            else:
                print("There was a problem with the chat. Please delete it and create a new one.")

        if choice == '4':
            print("Select a chat to delete. Changes are irreversible.")
            chats = get_chats(connection, username, private_key)
            chatid1, user = print_chat_list(chats)
            delete_chat(connection, username, Sha512(private_pem), chatid1)

        if choice == '5':
            delete_user(connection, username, private_pem)

        choice = '0'
       
    print("Program Ended.")

main()
```
