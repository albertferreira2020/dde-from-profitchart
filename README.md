# dde-from-profitchart
This is a api rest in node.js vendor trade data from profitchart software.

```javascript
const client = require('ts-dde');
const compression = require('compression')
const express = require('express');
const bodyParser = require('body-parser'); 
const app = express();
const http = require('http');
  

app.use(function(req, res, next) {
  res.header('Access-Control-Allow-Origin', "*");
  res.header('Access-Control-Allow-Methods','GET,PUT,POST,DELETE');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
  });

      
  
  app.use(compression());
  app.disable('x-powered-by');
  
  app.use(bodyParser.urlencoded({ extended: false }));
  app.use(bodyParser.json());

  //carrega página principal
  app.use(express.static(__dirname + '/'));
  
  app.get('/', function(req,res){
      console.log (__dirname);
      res.sendFile(__dirname + '/front.html');
  });

 
http.createServer(app).listen(5000,() => console.log('listening on http://localhost:5000/'));

 
const { Console } = require('console');

var ativo = '[R] WINZ20'
var DAT,HOR,ULT,ABE,MAX,MIN,FEC,PEX,VAR,VARPTS,MED,NEG,QTT,VOL,OCP,OVD,VOC,VOV,SEM,MES,TRESM,SEISM,DOZEM,ANO,TRIM,SEMES,PERMAX,PERMIN,C20, C40, C60, C80, C100, OUTPUT
 
var client1 = new client.DdeClient({
    services: {
      profitchart: {
        cot: [ativo+'.DAT',ativo+'.HOR',ativo+'.ULT',ativo+'.ABE',ativo+'.MAX',ativo+'.MIN',ativo+'.VAR',ativo+'.VARPTS',ativo+'.MED',ativo+'.NEG',ativo+'.QTT',ativo+'.VOL',ativo+'.OCP',ativo+'.OVD',ativo+'.VOC',ativo+'.VOV',ativo+'.SEM',ativo+'.MES',ativo+'.3M',ativo+'.6M',ativo+'.12M',ativo+'.ANO',ativo+'.TRIM',ativo+'.SEMES'],
      },
    },
  });

 

  client1.on('advise', (data) => {

    if (data.item == ativo+'.DAT' && DAT != data.text){DAT = data.text}
    if (data.item == ativo+'.HOR' && HOR != data.text){HOR = parseFloat(parseInt(data.text.replace(/:/g,'')) /1000000)  }
    if (data.item == ativo+'.ULT' && ULT != data.text){ULT = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.ABE' && ABE != data.text){ABE = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.MAX' && MAX!= data.text){MAX = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.MIN' && MIN!= data.text){MIN = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.VAR' && VAR!= data.text){VAR = parseFloat(data.text.replace(/,/g,'.'))}
    if (data.item == ativo+'.VARPTS' && VARPTS!= data.text){VARPTS =parseFloat(parseInt(data.text)/10000)}
    if (data.item == ativo+'.MED' && MED!= data.text){MED = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.NEG' && NEG!= data.text){NEG = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.QTT' && QTT!= data.text){QTT = parseFloat(parseInt(data.text)/10000000)}
    if (data.item == ativo+'.VOL' && VOL!= data.text){VOL = parseFloat(parseInt(data.text)/100000000000)}
    if (data.item == ativo+'.OCP' && OCP!= data.text){OCP = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.OVD' && OVD!= data.text){OVD = parseFloat(parseInt(data.text)/1000000)}
    if (data.item == ativo+'.VOC' && VOC!= data.text){VOC = parseFloat(parseInt(data.text)/10000)}
    if (data.item == ativo+'.VOV' && VOV!= data.text){VOV = parseFloat(parseInt(data.text)/10000)}
    if (data.item == ativo+'.SEM' && SEM!= data.text){SEM = parseFloat(parseInt(data.text)/10)}
    if (data.item == ativo+'.MES' && MES!= data.text){MES = parseFloat(parseInt(data.text)/100)}
    if (data.item == ativo+'.3M' && TRESM!= data.text){TRESM = parseFloat(parseInt(data.text)/100)}
    if (data.item == ativo+'.6M' && SEISM!= data.text){SEISM =  parseFloat(parseInt(data.text)/100)}
    if (data.item == ativo+'.12M' && DOZEM!= data.text){DOZEM =  parseFloat(parseInt(data.text)/10)}
    if (data.item == ativo+'.ANO' && ANO!= data.text){ANO =  parseFloat(parseInt(data.text)/100)}
    if (data.item == ativo+'.TRIM' && TRIM!= data.text){TRIM =  parseFloat(parseInt(data.text)/10)}
    if (data.item == ativo+'.SEMES' && SEMES!= data.text){SEMES =  parseFloat(parseInt(data.text)/10)}
 
    console.log(HOR +',' + ULT+',' +ABE+',' +MAX+',' +MIN+',' +VAR+',' +VARPTS+',' +MED+',' +NEG+',' +QTT+',' +VOL+',' +OCP+',' +OVD+',' +VOC+',' +VOV+',' +SEM+',' +MES+',' +TRESM+',' +SEISM+',' +DOZEM+',' +ANO+',' +TRIM+',' +SEMES)
 
})


 

  app.get('/dados', function (req, res) {
    res.json([{DAT,HOR,ULT,ABE,MAX,MIN,FEC,PEX,VAR,VARPTS,MED,NEG,QTT,VOL,OCP,OVD,VOC,VOV,SEM,MES,TRESM,SEISM,DOZEM,ANO,TRIM,SEMES,PERMAX,PERMIN,C20,C40,C60,C80,C100,OUTPUT}])
  });


  client1.connect();
  client1.startAdvise();
  setInterval(function() { client1.dispose(); }, 999999999);

 