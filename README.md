# College-admissions-<!DO
<!DOCTYPE html>
<html>
<head>
  <title>College Admission Portal</title>
  <script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getFirestore, collection, addDoc, getDocs, query, where } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
import { getStorage, ref, uploadBytes, getDownloadURL } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-storage.js";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const storage = getStorage(app);

window.showRole = function(){
  const role = document.getElementById("role").value;

  document.getElementById("studentBox").style.display = "none";
  document.getElementById("staffBox").style.display = "none";
  document.getElementById("collegeBox").style.display = "none";

  if(role === "student"){
    document.getElementById("studentBox").style.display = "block";
  }
  else{
    document.getElementById("staffBox").style.display = "block";
  }
}

window.saveStudent = async function(){

  const name = document.getElementById("sname").value;
  const mark = document.getElementById("smark").value;
  const district = document.getElementById("sdistrict").value;

  await addDoc(collection(db,"students"),{
    name:name,
    mark:mark,
    district:district
  });

  alert("Student Data Saved!");
  loadColleges(district);
}

window.addCollege = async function(){

  const file = document.getElementById("cphoto").files[0];
  const storageRef = ref(storage,"collegePhotos/"+file.name);

  await uploadBytes(storageRef,file);
  const url = await getDownloadURL(storageRef);

  await addDoc(collection(db,"colleges"),{
    name:document.getElementById("cname").value,
    district:document.getElementById("cdistrict").value,
    course:document.getElementById("ccourse").value,
    photo:url
  });

  alert("College Added!");
}

async function loadColleges(district){

  document.getElementById("collegeBox").style.display="block";

  const q = query(collection(db,"colleges"),where("district","==",district));
  const snapshot = await getDocs(q);

  document.getElementById("collegeList").innerHTML="";

  snapshot.forEach(doc=>{
    const data = doc.data();
    document.getElementById("collegeList").innerHTML+=`
      <h4>${data.name}</h4>
      <img src="${data.photo}" width="100"><br>
      ${data.course}<hr>
    `;
  });
}

  </script>

  <style>
    body{
      font-family:Arial;
      text-align:center;
      background:#f2f2f2;
    }
    .box{
      background:white;
      padding:20px;
      margin:20px auto;
      width:320px;
      border-radius:10px;
      box-shadow:0 0 10px #ccc;
    }
    input,select,button{
      margin:5px;
      padding:8px;
      width:90%;
    }
  </style>
</head>

<body>

<h2>Arts & Science College Admission Portal</h2>

<div class="box">
  <h3>Select Role</h3>
  <select id="role" onchange="showRole()">
    <option value="">--Select--</option>
    <option value="student">Student</option>
    <option value="staff">Staff</option>
  </select>
</div>

<div class="box" id="studentBox" style="display:none;">
  <h3>Student Details</h3>
  <input type="text" id="sname" placeholder="Name"><br>
  <input type="number" id="smark" placeholder="12th Mark"><br>
  <select id="sdistrict">
    <option>Chennai</option>
    <option>Coimbatore</option>
    <option>Madurai</option>
    <option>Trichy</option>
  </select><br>
  <button onclick="saveStudent()">Submit</button>
</div>

<div class="box" id="staffBox" style="display:none;">
  <h3>Add College</h3>
  <input type="text" id="cname" placeholder="College Name"><br>
  <select id="cdistrict">
    <option>Chennai</option>
    <option>Coimbatore</option>
    <option>Madurai</option>
    <option>Trichy</option>
  </select><br>
  <input type="text" id="ccourse" placeholder="Courses"><br>
  <input type="file" id="cphoto"><br>
  <button onclick="addCollege()">Add College</button>
</div>

<div class="box" id="collegeBox" style="display:none;">
  <h3>Available Colleges</h3>
  <div id="collegeList"></div>
</div>

</body>
</html>
          
