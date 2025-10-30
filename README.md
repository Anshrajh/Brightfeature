<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>SVR Public School</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
  body {
    font-family: "Poppins", sans-serif;
    background: #f5f8ff;
    margin: 0;
  }
  header {
    background: #004aad;
    color: white;
    padding: 20px;
    text-align: center;
  }
  nav {
    display: flex;
    justify-content: center;
    background: #e8efff;
    gap: 15px;
    padding: 10px;
  }
  nav button {
    background: #004aad;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 6px;
    cursor: pointer;
    transition: 0.3s;
  }
  nav button:hover {
    background: #00347a;
  }
  section {
    display: none;
    padding: 20px;
  }
  section.active {
    display: block;
  }
  .card {
    background: white;
    border-radius: 10px;
    padding: 15px;
    margin: 10px auto;
    max-width: 700px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  }
  .chart-container {
    width: 90%;
    max-width: 600px;
    margin: 20px auto;
  }
  input, select {
    padding: 8px;
    margin: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  button.save {
    background: #28a745;
    color: white;
    border: none;
    padding: 8px 15px;
    border-radius: 5px;
    cursor: pointer;
  }
</style>
</head>
<body>
  <header>
    <h1>SVR Public School</h1>
    <p>Learn, Grow & Shine</p>
  </header>

  <nav>
    <button onclick="showSection('home')">Home</button>
    <button onclick="showSection('dashboard')">Dashboard</button>
    <button onclick="showSection('report')">Report Card</button>
    <button onclick="showSection('attendance')">Attendance</button>
    <button onclick="showSection('announcements')">Announcements</button>
  </nav>

  <section id="home" class="active">
    <div class="card">
      <h2>Welcome to SVR Public School</h2>
      <p>We focus on holistic education with technology-driven learning experiences. Use the navigation above to explore student dashboards, reports, and more.</p>
    </div>
  </section>

  <section id="dashboard">
    <div class="card">
      <h2>Dashboard Overview</h2>
      <div class="chart-container">
        <canvas id="progressChart"></canvas>
      </div>
    </div>
  </section>

  <section id="report">
    <div class="card">
      <h2>Student Report Card</h2>
      <input id="studentName" placeholder="Student Name" />
      <input id="math" placeholder="Math %" type="number" />
      <input id="science" placeholder="Science %" type="number" />
      <input id="english" placeholder="English %" type="number" />
      <button class="save" onclick="saveReport()">Save Report</button>
      <div id="reportOutput"></div>
    </div>
  </section>

  <section id="attendance">
    <div class="card">
      <h2>Attendance Tracker</h2>
      <input id="attendanceName" placeholder="Student Name" />
      <select id="attendanceStatus">
        <option value="Present">Present</option>
        <option value="Absent">Absent</option>
      </select>
      <button class="save" onclick="saveAttendance()">Save</button>
      <div id="attendanceList"></div>
    </div>
  </section>

  <section id="announcements">
    <div class="card">
      <h2>School Announcements</h2>
      <textarea id="announcementText" placeholder="Write new announcement..." style="width:90%;height:80px;"></textarea>
      <br />
      <button class="save" onclick="addAnnouncement()">Post</button>
      <div id="announcementList"></div>
    </div>
  </section>

  <script>
    // Section switching
    function showSection(id) {
      document.querySelectorAll('section').forEach(sec => sec.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    // Dashboard chart
    const ctx = document.getElementById('progressChart');
    new Chart(ctx, {
      type: 'bar',
      data: {
        labels: ['Math', 'Science', 'English'],
        datasets: [{
          label: 'Average Class Performance',
          data: [85, 78, 90],
          backgroundColor: ['#004aad','#28a745','#ffc107']
        }]
      },
      options: {
        responsive: true,
        scales: { y: { beginAtZero: true, max: 100 } }
      }
    });

    // Report card saving
    function saveReport() {
      const name = studentName.value;
      const mathVal = +math.value;
      const sciVal = +science.value;
      const engVal = +english.value;
      const avg = ((mathVal + sciVal + engVal) / 3).toFixed(2);
      const record = { name, mathVal, sciVal, engVal, avg };
      localStorage.setItem('report', JSON.stringify(record));
      showReport();
    }

    function showReport() {
      const data = JSON.parse(localStorage.getItem('report'));
      if (!data) return;
      reportOutput.innerHTML = `
        <h3>${data.name}</h3>
        <p>Math: ${data.mathVal}%</p>
        <p>Science: ${data.sciVal}%</p>
        <p>English: ${data.engVal}%</p>
        <strong>Average: ${data.avg}%</strong>
      `;
    }
    showReport();

    // Attendance
    function saveAttendance() {
      const name = attendanceName.value;
      const status = attendanceStatus.value;
      let list = JSON.parse(localStorage.getItem('attendance') || '[]');
      list.push({ name, status, date: new Date().toLocaleDateString() });
      localStorage.setItem('attendance', JSON.stringify(list));
      showAttendance();
    }

    function showAttendance() {
      const list = JSON.parse(localStorage.getItem('attendance') || '[]');
      attendanceList.innerHTML = list.map(i => `<p>${i.date}: ${i.name} - ${i.status}</p>`).join('');
    }
    showAttendance();

    // Announcements
    function addAnnouncement() {
      const text = announcementText.value.trim();
      if (!text) return;
      let list = JSON.parse(localStorage.getItem('announcements') || '[]');
      list.unshift({ text, date: new Date().toLocaleString() });
      localStorage.setItem('announcements', JSON.stringify(list));
      announcementText.value = '';
      showAnnouncements();
    }

    function showAnnouncements() {
      const list = JSON.parse(localStorage.getItem('announcements') || '[]');
      announcementList.innerHTML = list.map(a => `<p><strong>${a.date}</strong><br>${a.text}</p><hr>`).join('');
    }
    showAnnouncements();
  </script>
</body>
</html>
