# College-admissions-<!DO
<!DOCTYPE html>
<html>
<head>
  <title>Arts & Science College Admission</title>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-storage.js"></script>

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
      box-shadow: 0px 0px 10px gray;
    }

    input, select, button {
      margin: 6px;
      padding: 8px;
      width: 90%;
    }

    img {
      margin-top: 10px;
      border-radius: 8px;
    }
  </style>
</head>
<body>

<h1>Arts & Science College Admission Portal</h1>

<!-- STUDENT REGISTER -->
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

<!-- LOGIN -->
<div class="box">
  <h3>Login</h3>
  <input type="email" id="loginEmail" placeholder="Email"><br>
  <input type="password" id="loginPass" placeholder="Password"><br>
  <button onclick="login()">Login</button>
</div>

<!-- STAFF ADD COLLEGE -->
<div class="box">
  <h3>Staff Add College</h3>

  <input type="text" id="cname" placeholder="College Name"><br>

  <select id="cdistrict">
    <option>Chennai</option>
    <option>Coimbatore</option>
    <option>Madurai</option>
    <option>Trichy</option>
  </select><br>

  <input type="text" id="clocation" placeholder="Location"><br>
  <input type="number" id="cfees" placeholder="Semester Fees"><br>
  <input type="date" id="clastdate"><br>
  <input type="file" id="cphoto"><br>

  <button onclick="addCollege()">Add College</button>
</div>

<!-- COLLEGE DISPLAY -->
<div class="box">
  <h3>Colleges</h3>
  <div id="collegeList"></div>
</div>

<script>

  // 🔥 Replace with YOUR Firebase config
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

  // STUDENT REGISTER
  function studentRegister() {

    auth.createUserWithEmailAndPassword(
      semail.value,
      spass.value
    ).then((userCredential) => {

      db.collection("students").doc(userCredential.user.uid).set({
        name: sname.value,
        mark: smark.value,
        district: sdistrict.value.trim().toLowerCase(),
        email: semail.value
      });

      alert("Student Registered Successfully!");
    });
  }

  // LOGIN
  function login() {
    auth.signInWithEmailAndPassword(
      loginEmail.value,
      loginPass.value
    ).then(() => {
      alert("Login Success!");
      loadColleges();
    });
  }

  // ADD COLLEGE
  function addCollege() {

    const file = cphoto.files[0];
    const storageRef = storage.ref("collegePhotos/" + file.name);

    storageRef.put(file).then(() => {
      storageRef.getDownloadURL().then((url) => {

        db.collection("colleges").add({
          name: cname.value,
          district: cdistrict.value.trim().toLowerCase(),
          location: clocation.value,
          fees: cfees.value,
          lastdate: clastdate.value,
          photo: url
        });

        alert("College Added Successfully!");
      });
    });
  }

  // LOAD COLLEGES BASED ON STUDENT DISTRICT
  function loadColleges() {

    auth.onAuthStateChanged(user => {
      if(user){

        db.collection("students").doc(user.uid).get().then(doc => {

          const studentDistrict = doc.data().district;

          db.collection("colleges").get().then(snapshot => {

            collegeList.innerHTML = "";

            snapshot.forEach(doc => {

              const data = doc.data();

              if(data.district === studentDistrict){

                collegeList.innerHTML += `
                  <h4>${data.name}</h4>
                  <img src="${data.photo}" width="150"><br>
                  <b>Location:</b> ${data.location}<br>
                  <b>Semester Fees:</b> ₹${data.fees}<br>
                  <b>Last Date:</b> ${data.lastdate}<br>
                  <hr>
                `;
              }

            });

          });

        });

      }
    });
  }

</script>

</body>
</html>
      
