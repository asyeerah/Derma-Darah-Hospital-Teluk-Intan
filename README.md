
<html lang="ms">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Kuiz Derma Darah + Sijil A4 Landscape</title>
  <style>
    body {
      margin:0;
      font-family:Arial,Helvetica,sans-serif;
      background:#0f172a;
      color:#f1f5f9;
      line-height:1.6
    }
    .container {
      max-width:900px;
      margin:0 auto;
      padding:20px
    }
    .card {
      background:#1e293b;
      padding:20px;
      border-radius:12px
    }
    h1{margin-top:0;font-size:clamp(20px,5vw,32px)}
    .question{margin-bottom:20px;padding:14px;border:1px solid #334155;border-radius:8px}
    .q-title{font-weight:bold;margin-bottom:10px;font-size:clamp(16px,4vw,20px)}
    .feedback{margin-top:8px;font-size:14px}
    .correct{color:#22c55e}
    .incorrect{color:#ef4444}
    button {
      padding:12px 18px;
      border:none;
      border-radius:8px;
      font-weight:bold;
      cursor:pointer;
      font-size:clamp(14px,4vw,18px);
      flex:1
    }
    .primary{background:#22d3ee;color:#0f172a}
    .secondary{background:#334155;color:#f1f5f9}
    .hidden{display:none}
    input[type="text"]{
      width:100%;
      padding:12px;
      border-radius:6px;
      border:1px solid #334155;
      margin-bottom:10px;
      background:#0b1220;
      color:#f1f5f9;
      font-size:clamp(14px,4vw,18px)
    }
    label.option {
      display:flex;
      align-items:center;
      gap:8px;
      padding:10px;
      border-radius:6px;
      border:1px solid #24303b;
      background:#0b1320;
      font-size:clamp(14px,4vw,18px)
    }
    label.option input{margin-right:8px}

    /* Certificate styling */
    #certificate {
      background:#fff;
      color:#000;
      text-align:center;
      padding:40px 20px;
      border:8px solid gold;
      border-radius:20px;
      width:100%;
      max-width:1123px;
      aspect-ratio:297/210;
      box-sizing:border-box;
      box-shadow:0 0 20px rgba(0,0,0,0.3);
      margin:20px auto
    }
    #certificate h2{font-size:clamp(22px,6vw,48px);margin-bottom:10px}
    #certificate h4{font-size:clamp(18px,4vw,28px);margin-top:0;margin-bottom:30px;font-style:italic}
    #certificate p{font-size:clamp(14px,4vw,22px);margin:12px 0}
    #certificate .name{font-size:clamp(20px,6vw,38px);font-weight:bold;margin:20px 0}
    #certificate h3{font-size:clamp(18px,5vw,30px);margin:20px 0}

    /* Print settings */
    @page{size:A4 landscape;margin:0}
    @media print {
      body{background:none}
      button{display:none}
      .card{display:none}
      #certificate{
        box-shadow:none;
        border-radius:0;
        width:100%;height:100%;
        max-width:none;
        aspect-ratio:auto;
        margin:0;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="card">
      <h1>Kuiz Derma Darah</h1>
      <section id="step-name">
        <input id="nama" type="text" placeholder="Masukkan nama penuh" />
        <button class="primary" id="btnStart">Mula Kuiz</button>
      </section>

      <section id="step-quiz" class="hidden">
        <div id="questionList"></div>
        <div style="display:flex;gap:8px;margin-top:10px;flex-wrap:wrap">
          <button class="secondary" id="btnReset">Set Semula</button>
          <button class="primary" id="btnSubmit">Hantar Jawapan</button>
        </div>
      </section>

      <section id="step-result" class="hidden">
        <h2>Keputusan</h2>
        <div id="scorePct" style="font-size:clamp(18px,5vw,28px);font-weight:bold"></div>
        <div id="scoreDetail" style="margin-bottom:10px"></div>
        <button class="primary" id="btnShowCert">Lihat Sijil</button>
      </section>
    </div>
  </div>

  <section id="step-certificate" class="hidden">
    <div id="certificate">
      <h2>Sijil Penyertaan Kuiz Derma Darah</h2>
      <h4>Hospital Teluk Intan</h4>

      <p>Disahkan bahawa</p>
      <div class="name" id="certName">[Nama Peserta]</div>
      <p>telah menyertai Kuiz Derma Darah dengan pencapaian</p>
      <h3 id="certScore">[Markah %]</h3>
      <p>Terima kasih atas penyertaan anda!</p>

      <button onclick="window.print()">Cetak / Simpan</button>
      <button class="secondary" id="btnRetake">Cuba Lagi</button>
    </div>
  </section>

<script>
const quiz=[
 {q:"Berapakah bacaan hemoglobin MINIMUM bagi melayakkan seorang penderma darah WANITA menderma darah ?",options:["13.5 g/dL","12.5 g/dL","12.0 g/dL"],answer:1,explain:"Wanita mesti mempunyai hemoglobin sekurang-kurangnya 12.5 g/dL."},
 {q:"Jika anda baru BERBEKAM, anda perlu menangguhkan pendermaan selama",options:["3 bulan","4 bulan","6 bulan"],answer:0,explain:"Selepas berbekam, perlu tunggu 3 bulan sebelum boleh menderma darah."},
 {q:"Umur MINIMUM bagi penderma darah ialah",options:["15 tahun","17 tahun","18 tahun"],answer:1,explain:"Penderma darah mestilah berumur sekurang-kurangnya 17 tahun."},
 {q:"Sekiranya anda mengalami DEMAM & SELESEMA, adakah anda boleh menderma darah ?",options:["BOLEH","TIDAK BOLEH"],answer:1,explain:"Jika demam/selesema, anda tidak dibenarkan menderma darah."},
 {q:"Anda disarankan untuk tidur MINIMUM 8 jam pada malam sebelum hadir menderma darah",options:["YA","TIDAK"],answer:1,explain:"Minimum tidur yang diperlukan pada malam sebelum menderma darah ialah 5 jam."},
 {q:"Apakah DOKUMEN yang perlu dibawa semasa hadir menderma darah ?",options:["Kad Pengenalan / Pasport","Kad Pekerja / Kad Pelajar","Lesen Memandu"],answer:0,explain:"Dokumen utama ialah Kad Pengenalan atau Pasport."},
 {q:"Anda disarankan untuk MAKAN DAN MINUM secukupnya dalam tempoh……………. sebelum menderma darah",options:["8 jam","10 jam","4 jam"],answer:2,explain:"Makan/minum secukupnya dalam tempoh 4 jam sebelum menderma darah."},
 {q:"Sekiranya anda mengalami PENING selepas menderma darah , apa yang wajar dilakukan?",options:["Teruskan aktiviti seperti biasa","Duduk berehat / baring dan tinggikan bahagian kaki, serta minum air kosong secukupnya","Berjoging"],answer:1,explain:"Jika pening, baringkan diri dan tinggikan kaki serta minum air."},
 {q:"KUMPULAN DARAH manusia terdiri daripada darah jenis",options:["A,B,C dan D","A,B, O dan AB","A,E,I,O dan U"],answer:1,explain:"Jenis kumpulan darah ialah A, B, O dan AB."},
 {q:"Tema sambutan Hari Kebangsaan Malaysia 2025 ialah",options:["Malaysia MADANI : Rakyat Disantuni","Malaysia MADANI : Jiwa Merdeka","Malaysia MADANI: Tekad Perpaduan Penuhi Harapan."],answer:0,explain:"Tema rasmi 2025 ialah Malaysia MADANI: Rakyat Disantuni."}
];

const stepName=document.getElementById('step-name');
const stepQuiz=document.getElementById('step-quiz');
const stepResult=document.getElementById('step-result');
const stepCert=document.getElementById('step-certificate');
const namaInput=document.getElementById('nama');
const btnStart=document.getElementById('btnStart');
const btnReset=document.getElementById('btnReset');
const btnSubmit=document.getElementById('btnSubmit');
const list=document.getElementById('questionList');
const scorePct=document.getElementById('scorePct');
const scoreDetail=document.getElementById('scoreDetail');
const btnShowCert=document.getElementById('btnShowCert');
const certName=document.getElementById('certName');
const certScore=document.getElementById('certScore');
const btnRetake=document.getElementById('btnRetake');

let finalPercent=0;

function renderQuiz(){
 list.innerHTML='';
 quiz.forEach((item,idx)=>{
  const qEl=document.createElement('div');qEl.className='question';
  const title=document.createElement('div');title.className='q-title';title.textContent=(idx+1)+'. '+item.q; qEl.appendChild(title);
  const opts=document.createElement('div');opts.className='options';
  item.options.forEach((opt,j)=>{
   const label=document.createElement('label');label.className='option';
   const input=document.createElement('input');input.type='radio';input.name='q'+idx;input.value=j;
   label.appendChild(input); label.appendChild(document.createTextNode(opt)); opts.appendChild(label);
  });
  qEl.appendChild(opts);
  const fb=document.createElement('div');fb.className='feedback hidden'; qEl.appendChild(fb);
  list.appendChild(qEl);
 });
}

btnStart.addEventListener('click',()=>{
 if(!namaInput.value.trim()){ alert('Sila masukkan nama.'); namaInput.focus(); return; }
 stepName.classList.add('hidden');
 renderQuiz();
 stepQuiz.classList.remove('hidden');
});

btnReset.addEventListener('click',()=>{ renderQuiz(); });

btnSubmit.addEventListener('click',()=>{
 let correct=0;
 document.querySelectorAll('#questionList .question').forEach((qEl,i)=>{
  const selected=qEl.querySelector('input[type=radio]:checked');
  const fb=qEl.querySelector('.feedback');
  fb.classList.remove('hidden');
  if(selected && parseInt(selected.value)===quiz[i].answer){
    correct++;
    fb.textContent='✅ Betul! '+quiz[i].explain;
    fb.className='feedback correct';
  } else {
    fb.textContent='❌ Salah. '+quiz[i].explain;
    fb.className='feedback incorrect';
  }
 });
 finalPercent=Math.round((correct/quiz.length)*100);
 scorePct.textContent=finalPercent+'%';
 scoreDetail.textContent=`Betul ${correct} daripada ${quiz.length} soalan.`;
 stepResult.classList.remove('hidden');
});

btnShowCert.addEventListener('click',()=>{
 stepResult.classList.add('hidden');
 stepQuiz.classList.add('hidden');
 stepCert.classList.remove('hidden');
 certName.textContent=namaInput.value.trim();
 certScore.textContent=finalPercent+'%';
 window.scrollTo({top:0,behavior:'smooth'});
});

btnRetake.addEventListener('click',()=>{
 stepCert.classList.add('hidden');
 stepResult.classList.add('hidden');
 stepQuiz.classList.add('hidden');
 stepName.classList.remove('hidden');
 namaInput.value='';
});
</script>
</body>
</html>
