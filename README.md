<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لوحة إدارة طلبات السحب — AYOUB AMIRI DZ</title>
  <style>
    body{font-family: "Tajawal", sans-serif;background:#070707;color:#fff;margin:0;padding:0}
    header{padding:16px;background:#0f0f0f;text-align:center;font-size:20px;color:#d4af37;font-weight:700}
    table{width:100%;border-collapse:collapse;margin:16px 0}
    th,td{border:1px solid #333;padding:12px;text-align:center}
    th{background:#1a1a1a;color:#d4af37}
    button{padding:6px 10px;border:none;border-radius:6px;background:#d4af37;color:#070707;font-weight:700;cursor:pointer}
  </style>
</head>
<body>
<header>لوحة إدارة طلبات السحب — AYOUB AMIRI DZ</header>

<table>
  <thead>
    <tr>
      <th>الاسم</th>
      <th>رقم Baridimob</th>
      <th>المبلغ (DZD)</th>
      <th>الحالة</th>
      <th>إجراءات</th>
    </tr>
  </thead>
  <tbody id="requests">
    <!-- سيتم ملء الطلبات من السيرفر -->
  </tbody>
</table>

<script>
async function fetchRequests(){
  try{
    const res = await fetch('/api/withdraws');
    const data = await res.json();
    const tbody = document.getElementById('requests');
    tbody.innerHTML = '';
    data.forEach((r,i)=>{
      const tr = document.createElement('tr');
      tr.innerHTML=`
        <td>${r.name}</td>
        <td>${r.phone}</td>
        <td>${r.amount}</td>
        <td>${r.status}</td>
        <td><button onclick="markPaid(${i})">تم الدفع</button></td>
      `;
      tbody.appendChild(tr);
    });
  }catch(e){console.error(e);}
}

async function markPaid(index){
  await fetch(`/api/withdraws/${index}`,{method:'POST'});
  fetchRequests();
}

fetchRequests();
</script>
</body>
</html>
