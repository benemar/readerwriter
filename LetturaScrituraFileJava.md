# Lettura e scrittura su file di testo

## File di testo e file binari

Nei files i dati vengono sempre immagazzinati come una sequenza di cifre binarie (sequenze di 0 e 1).
Tuttavia, in alcuni casi è necessario interpretare tali sequenze come caratteri, attraverso opportuni meccanismi di conversione
di queste sequenze binarie nelle corrispondenti sequenze di caratteri.
Per quanto riguarda nello specifico Java, il linguaggio
ha diverse classi che permettono di effettuare operazioni di lettura e scrittura su file di testo, ossia file che
vengono codificati in base allo standard ASCII o Unicode.



In java il package java.io mette a disposizione molte classi per gestire il flusso dati
distinguendo tra classi per gli stream di caratteri e classi per gestire quelli in formato testo.
La differenza è resa palese dal nome delle classi utilizzate:

* i nomi delle classi usate per la lettura / scrittura di stream di caratteri contengono
  rispettivamente la parola “reader/writer”;
* i nomi delle classi usate per la lettura / scrittura di stream di byte contengono le
  parole InputStream / OutputStream.

In entrambi i casi, l’interazione con uno stream avviene in tre fasi:

1. Apertura dello stream: soggetta alla gestione delle eccezioni;
2. Lettura/Scrittura;
3. Chiusura dell stream: la fase di chiusura è importante in quanto se non effettuata può
   comportare la perdita di dati.

## Lettura e scrittura su file di testo

Per la gestione del testo si può ricorrere a classi specializzate in questo compito.
In questo modo, anziché trattare i dati come byte così come si farebbe con le classi
InputStream e OutputStream, si possono trattare per quello che sono:
sequenze di stringhe e caratteri.
L'elaborazione diretta di file di testo, può risultare utile in molte occasioni. In questi casi,
infatti, il file può essere facilmente manipolato con un text editor (cosa che con un file binario
non si può fare)
Per quanto riguarda la classi che sono deputate alla gestione dei file di testo, tutte hanno come base le classi
astratte Reader e  Writer.
Sono il parallelo della InputStream e della OutputStream. Infatti sono simili (anche nei
metodi) solo che ragionano per caratteri anzichè per byte.
Le classi Reader/Writer sono la radice di tutte le classi basate sui flussi di caratteri.

## Le classi astratte Reader e Writer

La gerarchia delle classi deputate alla lettura e scrittura di flussi di dati testuali è alquanto articolata.
Tuttavia il tutto si può schematizzare nelle seguenti relazioni tra le varie  classi:

 (![Readerwriter](readerWriter.jpeg))

### La classe astratta Writer

È una classe astratta per scrivere un flusso (stream)  di caratteri analogo a OutputStream ma che
ha come obiettivo quello di essere utilizzato con caratteri anzichè byte.
Poiché Writer è una classe astratta, non è utile di per sé, in quanto solo attraverso il procedimento
dell'ereditarietà, le sue sottoclassi, ridefinendone i metodi,
possono essere utilizzate per gestire uno stream di scrittura di caratteri.
La classe Writer, quindi in quanto astratta,  fornisce diversi metodi implementati dalle sue sottoclassi.

I principali metodi per gestire gli stream di scrittura di caratteri, definiti nella classe
astratta Writer, sono:


| Tipo metodo                                                                  | Descrizione                                                                                                                                 |
| ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| public void write(int c) throws IOException                                  | Scrive c sullo stream, sotto forma di carattere (vengono presi in considerazione   solo i 16 bit meno significativi)                        |
| public abstract void write(char[] buf, int offset, int c) throws IOException | Scrive sullo stream i c caratteri contenuti nell’array buf a partire dalla posizione   offset, cioè da buf[offset] a buf[offset + c - 1]. |
| public void write(char[] buf) throws IOException                             | Scrive tutti i caratteri contenuti nell’array buf. Corrisponde a write(buf, 0,  buf.length).                                               |
| public void write(String s, int offset, int c) throws IOException            | Scrive i c caratteri contenuti nella stringa s alle posizioni da offset a offset +  c - 1.                                                  |
| public void write(String s) throws IOException                               | Scrive tutti i caratteri della stringa s. Equivale a write(s, 0, s.length()).                                                               |
| public abstract void flush() throws IOException                              | Forza la scrittura effettiva sullo stream di eventuali caratteri che, per motivi di   efficienza, sono stati solo memorizzati in un buffer. |
|                                                                              | public abstract void close() throws IOException.                                                                                            |

Le principali classi derivate da Writer sono:

* OutputStreamWriter
* FileWriter
* BufferedWriter
* CharArrayWriter
* StringWriter
* PrintWriter
* FilterWriter
* PipedWriter

## La classe OutputStreamReader

Questa classe permette alle classi derivate da Writer
di utilizzare in output dei flussi di byte, trasformando
internamente in byte i caratteri trasmessi. In pratica essa fa da ponte tra un flusso di caratteri e
un flusso di byte.
Questa classe eredita da Writer i metodi astratti e li implementa.
Inoltre il costruttore di questa classe consente di passare vari parametri, tra i quali il font
di caratteri da usare.
I costruttori diponibili per questa classe sono:


| Costruttore                                              | Descrizione                                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| OutputStreamWriter(OutputStream out)                     | Crea un oggetto  OutputStreamWriter che usa il carattere di encoding di default.    |
| OutputStreamWriter(OutputStream out, Charset cs)         | Crea un OutputStreamWriter tche usa il charset dato .                               |
| OutputStreamWriter(OutputStream out, CharsetEncoder enc) | Crea un OutputStreamWriter che usa il come odifica il set di caratteri specificato. |
| OutputStreamWriter(OutputStream out, String charsetName) | Crea un  OutputStreamWriter che usa il set di caratteri passato come stinga.        |

I metodi della classe sono:


| Modificatore e tipo | Metodo e descizione                  | Azione                                                                        |
| --------------------- | -------------------------------------- | ------------------------------------------------------------------------------- |
| void                | close()                              | Chiude il flusso, scaricandolo prima                                          |
| void                | flush()                              | Svuota il flusso.                                                             |
| String              | getEncoding()                        | Restituisce il nome della codifica dei caratteri utilizzata da questo flusso. |
| void                | write(char[] cbuf, int off, int len) | Scrive una parte di una matrice di caratteri.                                 |
| void                | write(int c)                         | Scrive un singolo carattere.                                                  |
| void                | write(String str, int off, int len)  |                                                                               |

Si tratta di una classe che, pur nella sua semplice gestione, fa da ponte tra un flusso in uscita
e una sottoclasse di Writer. Infatti spesso questa classe, pur avendo la possibilità di essere autonoma,
si interpone tra altre classi di tipo Writer.

## La classe FileWriter

La classe FileWriter **consente di effettuare processi di scrittura su file**. Normalmente è una classe
che può essere usata quando si tratta di file di testo di piccole dimensioni, in quanto
non possiamo ancora modificare la dimensione del buffer di byte che  è impostata su 8192.
I caratteri vengono scritti sul file uno alla volta.
FileWriter è in pratica  un OutputStreamWriter specializzato per la scrittura di file di caratteri .
Non espone nuovi metodi in quanto usa le funzioni  ereditate dalle classi OutputStreamWriter e Writer.


| Constructor                                  | Description                                                                                                                                       |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| FileWriter (File file)                       | Costruisce un oggetto di tipo FileWriter dato un oggetti di classe File.                                                                          |
| FileWriter (FileDescriptor fd)               | Costruisce un oggetto FileWriter associato a un descrittore di file                                                                               |
| FileWriter (File file, boolean append)       | Costruisce un oggetto di tipo FileWriter dato un oggetti di classe File ,  con un valore booleano che indica se aggiungere o meno i dati scritti. |
| FileWriter (String fileName)                 | Costruisce un oggetto FileWriter a cui viene assegnato un nome file.                                                                              |
| FileWriter (String fileName, boolean append) | Costruisce un oggetto FileWriter a cui viene assegnato un nome file con un valore booleano che indica se aggiungere o meno i dati scritti.        |

Espone i seguenti metodi ereditati dalla classe OutputStreamWriter

> [ append, close, flush, getEncoding, write, write, write]

e metodi ereditati dalla classe astratta Writer

> [append, write]

Per tanto, i metodi di base di tale classe sono:


| Metodo                                       | Descrizione                                                                        |
| ---------------------------------------------- | ------------------------------------------------------------------------------------ |
| void write(String text)                      | E' usato per scrivere una stringa all'interno di un oggetto FileWriter             |
| void write(char c)                           | E' usato per scrivere una carattere all'interno di un  FileWriter                  |
| void write(char[] c)                         | E' usato per scrivere un array di caratteri dentro un  FileWriter.                 |
| void flush()                                 | Viene utilizzato per svuotare i dati di FileWriter.                                |
| void close()                                 | hiude il  FileWriter.                                                              |
| flush()                                      | vuota il flusso di dati in attesa di scrittura                                     |
| getEncoding()                                | Restituisce il nome della codifica dei caratteri utilizzata dallo specifico flusso |
| write(char[] cbuf, int off, int len)         | Scrive una parte dell'array di caratteri                                           |
| write(int c)                                 | Scrive un singolo carattere.                                                       |
| write(String str, int off, int len)          | Scrive una parte di una stringa                                                    |
| append(char c)                               | Aggiunge il carattere specificato nella scrittura del file.                        |
| append(CharSequence csq)                     | Aggiunge la sequenza di caratteri specificata .                                    |
| append(CharSequence csq, int start, int end) | Aggiunge la sottosequenza di caratteri specificata.                                |
| close()                                      | Chiude il flusso, dopo averlo svuotato in scrittura.                               |
| nullWriter()                                 | Restituisce un nuovo oggetto Writer che scarta tutti i caratteri.                  |
| write(char[] cbuf)                           | Scrive un array di caratteri.                                                      |
| write(String str)                            | Scrive una stringa.                                                                |

Si riporta un esempio di implementazione di un codice che fa uso di tale classe.

```java
import java.io.FileWriter;
import java.io.IOException;
import java.util.*;
class Main {
    public static void main(String[] args)
        throws IOException
    {
        // inizializza una stringa
        String str = "ABC";
        try {
  
            // attach a file to FileWriter
            FileWriter fw
                = new FileWriter("C:/data/file.txt");
  
            // legge ogni carattere dalla strinfa e lo scrive
            // in un oggetto di tipo FileWriter
            for (int i = 0; i < str.length(); i++)
                fw.write(str.charAt(i));
  
            System.out.println("Scrittura effettuata correttamente");
  
            // chiude il file
            fw.close();
        }
        catch (Exception e) {
            e.getStackTrace();
        }
    }
}
```

### La classe astratte Reader

Reader è una classe astratta, che fa parte come la Writer del package java.io, il
cui scopo è quello di rappresentare un flusso di caratteri. Anche questa classe, essendo
astratta, non può essere usata direttamente per creare oggetti di tale "tipo", ma
può essere utilizzata dalle sottoclassi cle la estendono.
Le sottoclassi di Reader sono:

* BufferedReader
* InputStreamReader
* FileReader
* StringReader

I costruttori principali di Reader sono:


| Costruttore                   | Descrizione                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------------- |
| FileReader(File file)         | Questo costruttore crea un nuovo FileReader, dato un oggetto  File da cui leggere         |
| FileReader(FileDescriptor fd) | Questo costruttore crea un nuovo FileReader, dato un descrittore  da cui leggere .        |
| FileReader(String fileName)   | Questo costruttore crea un nuovo FileReader, dato una stringa con il path e nome del file |

Per quanto riguarda i metodi, la classe è provvista di diversi metodi che devono
essere implementati nelle sottoclassi.
I metodi più comuni sono:


| Metodo                                    | Descrizione                                                                                                              |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| ready()                                   | Controlla se il lettore è pronto per essere letto                                                                       |
| read(char[] array)                        | Legge i caratteri dal flusso e li archivia nell'array specificato                                                        |
| read(char[] array, int start, int length) | Legge il numero di caratteri dalla lunghezza dal flusso dati e lo memorizza nell'array specificato a partire dall'inizio |
| mark()                                    | Contrassegna la posizione  fino a dove sono stati letti i dati                                                           |
| reset()                                   | Riporta il controllo nel punto del flusso in cui è impostato il contrassegno                                            |
| skip()                                    | Elimina il numero di caratteri specificato dal flusso                                                                    |

### La classe InputStreamReader

La classe converte i dati da byte in caratteri ed estende la classe Reader, facendo anche essa
da ponte tra i flussi di byte e i flussi di caratteri, andando  a leggere i byte come
caratteri. In altre parole, InputStreamReaderinterpreta
i byte in entrata come testo anziché come dati numerici.
InputStreamReader viene spesso utilizzata per leggere i caratteri provenienti da
connessioni di rete in cui i byte rappresentano il testo.

La classe è provvista di diversi costruttori.


| Nome Costruttore                                      | Descrizione                                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| InputStreamReader(InputStream in)                     | Crea un InputStreamReader che utilizza il set di caratteri predefinito.             |
| InputStreamReader(InputStream in, Charset cs)         | Crea un InputStreamReader che utilizza il set di caratteri specificato.             |
| InputStreamReader(InputStream in, CharsetDecoder dec) | Crea un InputStreamReader che utilizza il decodificatore  di caratteri specificato. |
| InputStreamReader(InputStream in, String charsetName) | Crea un InputStreamReader che utilizza il decodificatore  di caratteri denominato.  |

e diversi metodi:


| Modifier and Type                             | Method	Description                                                         |
| ----------------------------------------------- | ---------------------------------------------------------------------------- |
| void	close()                                  | Chiude il flusso e rilascia tutte le risorse di sistema ad esso associate. |
| String	getEncoding()                          | Restituisce il nome della codifica dei caratteri utilizzata dal flusso.    |
| int	read()                                    | Legge un singolo carattere.                                                |
| int	read(char[] cbuf, int offset, int length) | Legge i caratteri èartendo da una specificata posizione dell'array.       |
| boolean	ready()                               | Indica se il flusso è pronto per essere letto.                            |

Ad esempio una possibile scrittura di codice con tale classe potrebbe essere la seguente.

```java
public class InputStreamReaderExample {  
    public static void main(String[] args) {  
        try  {  
            InputStream stream = new FileInputStream("file.txt");  
            Reader reader = new InputStreamReader(stream);  
            int data = reader.read();  
            while (data != -1) {  
                System.out.print((char) data);  
                data = reader.read();  
            }  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
    }  
}  
```

Oppure

```java
import java.io.InputStreamReader;
import java.io.FileInputStream;

class Main {
  public static void main(String[] args) {

    // Crea un array di caratteri
    char[] array = new char[100];

    try {
      // Crea un FileInputStream
      FileInputStream file = new FileInputStream("input.txt");

      // Crea un  InputStreamReader
      InputStreamReader input = new InputStreamReader(file);

      // Kegge caratteri dal file
      input.read(array);
      System.out.println("Dati nel file:");
      System.out.println(array);

      // Chiude il lettore
      input.close();
    }

    catch(Exception e) {
      e.getStackTrace();
    }
  }
}
```

### La classe FileReader

Classe FileReader eredita dalla classe InputStreamReader (e indirettamente dalle Rader) costruttori
e metodi.
I caratteri vengono letti dal file uno alla volta.

I costruttori ereditati di cui è dotata la classe sono:


| Costruttore                                  | Descrizione                                                                                                                |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| FileReader(File file)                        | Crea un nuovo FileReader, dato il File da leggere, usando il set di caratteri predefinito della piattaforma.               |
| FileReader(FileDescriptor fd)                | Crea un nuovo FileReader, dato il FileDescriptor da leggere, usando il set di caratteri predefinito della piattaforma.     |
| FileReader(File file, Charset charset)       | Crea un nuovo FileReader, dato il File da leggere e il set di caratteri.                                                   |
| FileReader(String fileName)                  | Crea un nuovo FileReader, dato il nome del file da leggere, utilizzando il set di caratteri predefinito della piattaforma. |
| FileReader(String fileName, Charset charset) | Crea un nuovo FileReader, dato il nome del file da leggere e il charset.                                                   |

Mentre i metodi  sono:
Metodi ereditati da InputStreamReader

> [getEncoding, read, ready]

Metodi ereditati da Reader

> [close, mark, markSupported, nullReader, read, reset, skip, transferTo]

Per quanto riguarda il metodo **Read()** esso può essere implementato in diverso modo:


| Implementazione di Read()                 | Descrizione                                                                                                       |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| read()                                    | legge un singolo carattere dal lettore                                                                            |
| read(char[] array)                        | legge i caratteri dal lettore e li memorizza nell'array specificato                                               |
| read(char[] array, int start, int length) | legge dal lettore il numero di caratteri e lo memorizza nell'array specificato a partire dalla posizione iniziale |

Una possibile implementazione della classe FileReader usando il metodo
read che legge i dati passati in un array potrebbe essere, ad esempio:

```java
import java.io.FileReader;

class Main {
  public static void main(String[] args) {

    // Crea un array di  caratteri
    char[] array = new char[100];

    try {
      // Crea e legge usando FileRader
      FileReader input = new FileReader("input.txt");

      // Legge i caratteri
      input.read(array);
      System.out.println("Data in the file: ");
      System.out.println(array);

      // Chiude il lettore
      input.close();
    }

    catch(Exception e) {
      e.getStackTrace();
    }
  }
}
```

Un'altra possibile implementazione del metodo può essere:

```java
import java.io.FileReader;
import java.io.IOException;

public class FileReaderExample
{
public static void main(String[] args) throws IOException
{
String fileName = "demo.txt";

    FileReader fileReader = new FileReader(fileName);
 
    try {
         int i;
           while((i = fileReader.read()) != -1) {
            System.out.print((char)i);
           }
    } finally {
      fileReader.close();
    }
}
}
```

In questo esempio, usiamo il metodo read() che legge un singolo carattere dal file e lo restituisce.
Quando tutto il contenuto del file è stato letto, restituisce -1  (che indica la fine del file).
