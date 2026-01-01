# student-notes
Free notes website for class 9 and 10 students
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Student Notes</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: "Segoe UI", Tahoma, sans-serif;
  background: linear-gradient(135deg, #006af5, #2c0363);
 
}

/* HEADER */
header {
  background: linear-gradient(135deg, #6eecdc, #0068cf);
  color: white;
  padding: 25px 15px;
  text-align: center;
}

header h1 {
  margin: 0;
  font-size: 28px;
}

header p {
  margin-top: 5px;
  opacity: 0.9;
}

/* CONTAINER */
.container {
  padding: 25px;
  max-width: 1000px;
  margin: auto;
}

/* FILTERS */
.filters {
  display: flex;
  gap: 12px;
  margin-bottom: 25px;
  flex-wrap: wrap;
}

input, select {
  padding: 12px 14px;
  font-size: 15px;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
  transition: all 0.3s ease;
}

input:focus, select:focus {
  border-color: #3498db;
  box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
}

/* NOTES SECTION */
.notes {
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.08);
}

.notes h2 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #2c3e50;
}

/* NOTE CARD */
.note {
  background: #f9fbfd;
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 12px;
  border-left: 5px solid #3498db;
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  animation: fadeIn 0.4s ease;
}

.note:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 20px rgba(0,0,0,0.12);
}

.note strong {
  color: #6222f8;
  font-size: 16px;
}

/* BUTTON (future use) */
button {
  padding: 10px 14px;
  background: #3498db;
  color: rgb(255, 255, 255);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.2s ease;
}

button:hover {
  background: #24d61e;
  transform: scale(1.05);
}

/* ANIMATION */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(6px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* RESPONSIVE */
@media (max-width: 600px) {
  header h1 {
    font-size: 22px;
  }

  .filters {
    flex-direction: column;
  }
}
/* ADVANCED SKELETON CARD */
.skeleton {
  background: #f1f1f1;
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 12px;
  position: relative;
  overflow: hidden;
  animation: fadeOut 0.3sec ; 
  transition: ease-in-out  1.3;
 }

.skeleton::after {
  content: "";
  position: absolute;
  top: 0;
  left: -150%;
  height: 100%;
  width: 150%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.7),
    transparent
  );
  animation: shimmer 1.3s infinite;
}

@keyframes shimmer {
  0% { left: -150%; }
  100% { left: 150%; }
}

/* Skeleton inner lines */
.skeleton-title {
  height: 14px;
  width: 60%;
  background: #ddd;
  border-radius: 6px;
  margin-bottom: 10px;
}

.skeleton-text {
  height: 12px;
  width: 80%;
  background: #ddd;
  border-radius: 6px;
}

</style>

</head>

<body>

<header>
  <h1>Student Notes</h1>
  <p>Free notes for Class 9 & 10</p>
</header>

<div class="container">

  <!-- SEARCH & FILTER -->
  <div class="filters">
    <input type="text" id="searchInput" placeholder="Search notes...">
    
    <select id="classFilter">
      <option value="">All Classes</option>
      <option value="9">Class 9</option>
      <option value="10">Class 10</option>
    </select><select id="subjectFilter">
  <option value="">All Subjects</option>
  <option value="Maths">Maths</option>
  <option value="Science">Science</option>
  <option value="English">English</option>
</select>
  </div>


  <!-- NOTES LIST -->
  <div class="notes">
    <h2>Available Notes</h2>
    <div id="notesContainer"></div>
  </div>

</div>


<script>
/* ===============================
   NOTES DATA (DATABASE FOR NOW)
================================ */
const notes = [
  { 
    title: "Polynomials", 
    subject: "Maths", 
    class: "9", 
    pdf: "assets/class 9/bstchapter_2_notes_11.pdf"
  },
  { 
    title: "Linear Equations", 
    subject: "Maths", 
    class: "10", 
    pdf: "notes/linear_equations.pdf" 
  },
  { 
    title: "Chemical Reactions", 
    subject: "Science", 
    class: "10", 
    pdf: "notes/chemical_reactions.pdf" 
  }
];

/* ===============================
   ELEMENTS
================================ */
const notesContainer = document.getElementById("notesContainer");
const searchInput = document.getElementById("searchInput");
const classFilter = document.getElementById("classFilter");
const subjectFilter = document.getElementById("subjectFilter");

/* ===============================
   DISPLAY NOTES
================================ */
function displayNotes(filteredNotes) {
  notesContainer.innerHTML = "";

  if (filteredNotes.length === 0) {
    notesContainer.innerHTML = "<p>No notes found</p>";
    return;
  }

  filteredNotes.forEach(note => {
    const div = document.createElement("div");
    div.className = "note";

    div.innerHTML = `
      <strong>Class ${note.class} ${note.subject}</strong><br>
      Chapter: ${note.title}<br><br>

      <iframe 
        src="${note.pdf}"
        width="100%"
        height="450"
        style="border:1px solid #ccc; border-radius:8px;"
      ></iframe>

      <br><br>
      <button onclick="openPDF('${note.pdf}')">
        ⬇ Download PDF
      </button>
    `;

    notesContainer.appendChild(div);
  });
}


/* ===============================
   FILTER LOGIC
================================ */
function filterNotes() {
  const searchText = searchInput.value.toLowerCase();
  const selectedClass = classFilter.value;
  const selectedSubject = subjectFilter.value;

  showSkeleton();

  setTimeout(() => {
    const filtered = notes.filter(note => {

      if (selectedClass && note.class !== selectedClass) return false;
      if (selectedSubject && note.subject !== selectedSubject) return false;

      if (
        searchText &&
        !note.title.toLowerCase().includes(searchText) &&
        !note.subject.toLowerCase().includes(searchText)
      ) {
        return false;
      }

      return true;
    });

    displayNotes(filtered);
  }, 600);
}



/* ===============================
   EVENTS
================================ */
searchInput.addEventListener("input", filterNotes);
classFilter.addEventListener("change", filterNotes);
subjectFilter.addEventListener("change", filterNotes);

/* ===============================
   INITIAL LOAD
================================ */
showSkeleton();

setTimeout(() => {
  displayNotes(notes);
}, 800);


// functions (skeleton loading)
function showSkeleton(count = 4) {
  notesContainer.innerHTML = "";

  for (let i = 0; i < count; i++) {
    const div = document.createElement("div");
    div.className = "skeleton";
    div.innerHTML = `
      <div class="skeleton-title"></div>
      <div class="skeleton-text"></div>
    `;
    notesContainer.appendChild(div);
  }
}
// displayNotes() 

function displayNotes(filteredNotes) {
  notesContainer.innerHTML = "";

  if (filteredNotes.length === 0) {
    notesContainer.innerHTML = "<p>No notes found</p>";
    return;
  }

  filteredNotes.forEach(note => {
    const div = document.createElement("div");
    div.className = "note";

    div.innerHTML = `
      <strong>Class ${note.class} ${note.subject}</strong><br>
      Chapter: ${note.title}<br><br>

      <button onclick="openPDF('${note.pdf}')">
        📄 View PDF
      </button>
    `;

    notesContainer.appendChild(div);
  });
}
function openPDF(pdfPath) {
  window.open(pdfPath, "_blank");
}



</script>

</body>
</html>
