# College-admissions-<!DO
<!DOCTYPE html>
<html>
<head>
  <title>Arts & Science College Admission</title>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage.js"></script>

  <style>
    body {
      font-family: Arial;
      background: url("yourimage.jpg");
      background-size: cover;
      text-align: center;
    }
    input, select, button {
      margin: 5px;
      padding: 8px;
    }
    .box {
      background: white;
      padding: 20px;
      margin: 20px auto;
      width: 300px;
      border-radius: 10px;
    }
  </style>
</head>
<body>

<h1>Arts & Science College Admission Portal</h1>

<div class="box">
  <h3>Student Register</h3>
  <input type="text" id="sname" placeholder="Name"><br>
  <input type="number" id="smark" placeholder="12th Mark"><br>
  <select id="sdistrict">
    <option>Chennai</option>
    <option>Coimbatore</option>
    <option>Madurai</option>
    <option>Trichy</option>
  </select><br>
  <input type="email" id="semail" placeholder="Email"><br>
  <input type="password" id="spass" placeholder="Password"><br>
  <button onclick="studentRegister()">Register</button>
</div>

<div class="box">
  <h3>Login</h3>
  <input type="email" id="loginEmail" placeholder="Email"><br>
  <input type="password" id="loginPass" placeholder="Password"><br>
  <button onclick="login()">Login</button>
</div>

<div class="box">
  <h3>Staff Add College</h3>
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

<div class="box">
  <h3>Colleges</h3>
  <div id="collegeList"></div>
</div>

<script>
  // 🔥 Replace with your Firebase config
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_BUCKET",
    messagingSenderId: "YOUR_ID",
    appId: "YOUR_APP_ID"
  };

  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();
  const db = firebase.firestore();
  const storage = firebase.storage();

  function studentRegister() {
    const email = semail.value;
    const pass = spass.value;

    auth.createUserWithEmailAndPassword(email, pass)
    .then((userCredential) => {
      db.collection("students").doc(userCredential.user.uid).set({
        name: sname.value,
        mark: smark.value,
        district: sdistrict.value,
        email: email
      });
      alert("Student Registered Successfully!");
    });
  }

  function login() {
    auth.signInWithEmailAndPassword(loginEmail.value, loginPass.value)
    .then(() => {
      alert("Login Success");
      loadColleges();
    });
  }

  function addCollege() {
    const file = cphoto.files[0];
    const storageRef = storage.ref("collegePhotos/" + file.name);

    storageRef.put(file).then(() => {
      storageRef.getDownloadURL().then((url) => {
        db.collection("colleges").add({
          name: cname.value,
          district: cdistrict.value,
          course: ccourse.value,
          photo: url
        });
        alert("College Added!");
      });
    });
  }

  function loadColleges() {
    auth.onAuthStateChanged(user => {
      if(user){
        db.collection("students").doc(user.uid).get().then(doc => {
          const studentDistrict = doc.data().district;

          db.collection("colleges")
          .where("district", "==", studentDistrict)
          .get()
          .then(snapshot => {
            collegeList.innerHTML = "";
            snapshot.forEach(doc => {
              const data = doc.data();
              collegeList.innerHTML += `
                <h4>${data.name}</h4>
                <img src="${data.photo}" width="100"><br>
                ${data.course}<hr>
              `;
            });
          });
        });
      }
    });
  }
</script>

</body>
</html>
