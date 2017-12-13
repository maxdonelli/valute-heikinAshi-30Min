# valute-heikinAshi-30Min
TS valute grafico heikin ashi 30 minut
{ TS - heikin ashi 30 minuti su valute datafeed LMAX

setup ingresso : se ho 3 candele consecutive in aumento ( discesa) compro (vendo) .

sono stati definiti orari standard per iniziare 9.30 e 16.30 , ovviamente andranno modificati / ottimizzati in funzione del sottostante.

Versione base da ottimizzare, si puo ottimizzare :

- ora ingresso
- ora max uscita
-giorno della settimana
- volatilita giorno precedente 
- profit e loss
- trailing stop
inoltre si possono inserire tutti i filtri che volete : - supertrend - media mobile (o diverse medie mobili) - indicatori vari - ecc.....

buon lavoro

PS : MOLTO IMPORTANTE inserire commissioni e slippage adeguati ( si rischia di lavorare per nulla!!)

}

once ClearPrintLog;

input: perdita(15), guadagno(0),numContratti(1),oraInizioAnalisi(900), oraFineAnalisi(1630);

vars : bool sonoAmercato(false);

if marketposition <> 0 then // se sono a mercato fisso loss e profit e imposto il flag per un solo trade/giorno

begin
sonoAmercato= True;
setstoploss(perdita);
 if guadagno > 0 then setprofittarget(guadagno);


end;
condition1 = time > oraInizioAnalisi and Time < oraFineAnalisi and sonoAmercato = False ; // condizioni per ingresso a mercato

//----------------- entro a mercato

if condition1 and marketposition = 0 then

		begin
			if close[1] > close[2]  and close > close[1] then buy numContratti share next bar at market  ; 
			
			if Close[1] < close[2]  and close < close[1] then sellshort numContratti share next bar at market;	
			

				
		end;
//if marketposition = 1 and Time > 2140 then sell numContratti share next bar at market; // chiusura a fine sessione
//if marketposition = -1 and Time > 2140 then buytocover numContratti share next bar at market;

if date <> date[1] then sonoAmercato= false ; // reset flag ingresso a mercato

setexitonclose;
