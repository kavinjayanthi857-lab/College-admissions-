# College-admissions-<!DO
<!DOCTYPE html>
<html>
<head>
  <title>College Admission Portal</title>
  <style>
    body {
      font-family: Arial;
      background: #f2f2f2;
      text-align: center;
    }
    .box {
      background: white;
      padding: 20px;
      margin: 20px auto;
      width: 320px;
      border-radius: 10px;
      box-shadow: 0 0 10px #ccc;
    }
    input, select, button {
      margin: 5px;
      padding: 8px;
      width: 90%;
    }
    img {
      margin-top: 5px;
    }
  </style>
</head>
<body>

<h2>Arts & Science College Admission Portal</h2>

<!-- Role Selection -->
<div class="box">
  <h3>Select Role</h3>
  <select id="role">
    <option value="student">Student</option>
    <option value="staff">Staff</option>
  </select>
</div>

<!-- Phone Login -->
<div class="box">
  <h3>Phone Login</h3>
  <input type="text" id="phone" placeholder="+91XXXXXXXXXX"><br>
  <div id="recaptcha-container"></div><br>
  <button onclick="sendOTP()">Send OTP</button><br><br>
  <input type="text" id="otp" placeholder="Enter OTP"><br>
  <button onclick="verifyOTP()">Verify OTP</button>
</div>

<!-- Student Details -->
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
  <button onclick="saveStudent()">Save</button>
</div>

<!-- Staff Add College -->
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

<!-- College List -->
<div class="box" id="collegeBox" style="display:none;">
  <h3>Colleges</h3>
  <div id="collegeList"></div>
</div>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getAuth, RecaptchaVerifier, signInWithPhoneNumber } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
import { getFirestore, doc, setDoc, getDoc, collection, addDoc, query, where, getDocs } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
import { getStorage, ref, uploadBytes, getDownloadURL } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-storage.js";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_BUCKET",
  messagingSenderId: "YOUR_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const storage = getStorage(app);

window.recaptchaVerifier = new RecaptchaVerifier(auth, 'recaptcha-container', {
  'size': 'normal'
});

let confirmationResult;
let currentUser;

// Send OTP
window.sendOTP = async function () {
  const phoneNumber = document.getElementById("phone").value;
  confirmationResult = await signInWithPhoneNumber(auth, phoneNumber, window.recaptchaVerifier);
  alert("OTP Sent!");
}

// Verify OTP
window.verifyOTP = async function () {
  const code = document.getElementById("otp").value;
  const result = await confirmationResult.confirm(code);
  currentUser = result.user;

  const role = document.getElementById("role").value;

  await setDoc(doc(db, "users", currentUser.uid), {
    phone: currentUser.phoneNumber,
    role: role
  });

  alert("Login Successful!");
  loadDashboard(role);
}

// Load Dashboard
async function loadDashboard(role) {

  if(role === "student"){
    document.getElementById("studentBox").style.display = "block";
    document.getElementById("collegeBox").style.display = "block";
  }
  else{
    document.getElementById("staffBox").style.display = "block";
  }
}

// Save Student
window.saveStudent = async function () {
  await setDoc(doc(db, "students", currentUser.uid), {
    name: document.getElementById("sname").value,
    mark: document.getElementById("smark").value,
    district: document.getElementById("sdistrict").value
  });

  alert("Student Data Saved!");
  loadColleges();
}

// Add College (Staff)
window.addCollege = async function () {

  const file = document.getElementById("cphoto").files[0];
  const storageRef = ref(storage, "collegePhotos/" + file.name);

  await uploadBytes(storageRef, file);
  const url = await getDownloadURL(storageRef);

  await addDoc(collection(db, "colleges"), {
    name: document.getElementById("cname").value,
    district: document.getElementById("cdistrict").value,
    course: document.getElementById("ccourse").value,
    photo: url
  });

  alert("College Added!");
}

// Load Colleges for Student
async function loadColleges(){

  const docSnap = await getDoc(doc(db, "students", currentUser.uid));
  const studentDistrict = docSnap.data().district;

  const q = query(collection(db, "colleges"), where("district", "==", studentDistrict));
  const querySnapshot = await getDocs(q);

  document.getElementById("collegeList").innerHTML = "";

  querySnapshot.forEach((doc) => {
    const data = doc.data();
    document.getElementById("collegeList").innerHTML += `
      <h4>${data.name}</h4>
      <img src="${data.photo}" width="100"><br>
      ${data.course}<hr>
    `;
  });
}

</script>

</body>
</html>
          
