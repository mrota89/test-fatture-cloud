# test-fatture-cloud 

### Una volta clonata la repository in locale, da terminale spostarsi all'interno della cartella del progetto ed installare le dipendenze:
```
npm install
```

### Per avviare il server locale:
```
npm run serve
```

### Link al server locale:
```
http://localhost:8080/
```

### Funzionalità interazione utente

- L'utente può premere su un singolo mese per filtrare i documenti di quel mese, oppure trascinare il mouse tenendo cliccato per selezionare più mesi assieme.  
Inoltre è possibile effettuare una selezione multipla di mesi non adiacenti tra loro, tenendo premuto il tasto Ctrl;

- il mese o i mesi selezionati sono individuabili dall'utente grazie all'evidenziazione della barra verde inferiore; 

- ogni nuova selezione comporta l’annullamento della selezione precedente;

- i mesi selezionati vengono visualizzati in una lista sottostante il componente.
