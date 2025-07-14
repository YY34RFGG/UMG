
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>UMG</title>
  <style>
    body {
      font-family: sans-serif;
      background: #f8f0fb;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #5e2a84;
      margin-bottom: 30px;
    }

    h2 {
      padding: 10px;
      background: #5e2a84;
      color: white;
      border-radius: 6px;
      margin-top: 40px;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 12px;
      margin-top: 10px;
    }

    .card {
      border-radius: 8px;
      padding: 15px;
      background-color: white;
      border: 2px solid #ccc;
      transition: all 0.3s ease;
      cursor: pointer;
      opacity: 0.4;
    }

    .card.highlight {
      border-color: #5e2a84;
      opacity: 1;
      background-color: #e5d6f1;
    }

    .card.completed {
      background-color: #d6d6d6;
      text-decoration: line-through;
      opacity: 0.6;
    }
  </style>
</head>
<body>
  <h1>Malla Curricular Interactiva – Urgencias Médicas y desastres </h1>
  <p style="text-align:center;">Haz clic en una materia para completarla. Se desbloquearán automáticamente las siguientes si cumples los prerrequisitos. Puedes volver a hacer clic para pruebas.</p>
  <div id="container"></div>

  <script>
    const semesters = [
      { title: "Primer Semestre", color: "#e6dcf4" },
      { title: "Segundo Semestre", color: "#e6dcf4" },
      { title: "Primer Verano", color: "#e6dcf4" },
      { title: "Tercer Semestre", color: "#e6dcf4" },
      { title: "Cuarto Semestre", color: "#e4e4fa" },
      { title: "Segundo Verano", color: "#e6dcf4" },
      { title: "Quinto Semestre", color: "#dcf5f2" },
      { title: "Sexto Semestre", color: "#fef1c7" },
      { title: "Tercer Verano", color: "#d7eafc" },
      { title: "Séptimo Semestre", color: "#ffe9d6" },
      { title: "Octavo Semestre", color: "#ececec" }
    ];

    const courses = [
      ["2700", "Anatomía Humana I (Lab.)", 3, [], 0],
      ["7016", "Fisiología Humana I (Lab.)", 3, [], 0],
      ["2721", "Bioquímica (Lab.)", 3, [], 0],
      ["7017", "Transporte Prehospitalario", 3, [], 0],
      ["7018", "Semiología Médica", 3, [], 0],
      ["4761", "Inglés 448A", 4, [], 0],
      ["7019", "Práctica Universitaria I", 3, [], 0],
      ["1", "Taller", 0, [], 0],
      ["2707", "Anatomía Humana II (Lab.)", 3, ["2700"], 1],
      ["7021", "Fisiología Humana II (Lab.)", 3, ["7016"], 1],
      ["7022", "Manejo de Vías Aéreas Básico", 3, ["2700", "7016"], 1],
      ["7023", "Evaluación y Manejo del Paciente Prehospitalario", 3, ["7016", "7018"], 1],
      ["7024", "Gestión de Riesgos a Desastres", 3, ["7017"], 1],
      ["4762", "Inglés 448B", 4, ["4761"], 1],
      ["7025", "Práctica Universitaria II", 4, ["7019", "2700"], 1],
      ["6121", "Seminario de Bioseguridad e IAAS (40 horas)", 0, [], 1],
      ["...", "Electiva", 3, [], 2],
      ["5337", "Aspectos Legales", 3, [], 2],
      ["7026", "Acondicionamiento Físico", 3, [], 2],
      ["7027", "Bioética", 3, [], 2],
      ["7028", "Urgencias Médicas Prehospitalarias I", 3, ["2707", "7021"], 3],
      ["7029", "Urgencias Quirúrgicas Prehospitalarias I", 3, ["7022", "7021"], 3],
      ["7030", "Psiquiatría en Emergencias y Desastres", 3, ["7023"], 3],
      ["7031", "Análisis y Reducción del Riesgo en Desastres", 3, ["7024"], 3],
      ["7032", "Búsqueda, Rescate y Extracción", 3, ["7024"], 3],
      ["4763", "Inglés 449A", 3, ["4762"], 3],
      ["7033", "Práctica Universitaria III", 5, ["7025", "7022"], 3],
      ["7034", "Urgencias Médicas Prehospitalarias II", 3, ["7028"], 4],
      ["7035", "Urgencias Quirúrgicas Prehospitalarias II", 3, ["7029"], 4],
      ["7036", "Atención de Emergencias QRVEN", 3, ["7032"], 4],
      ["7037", "Sist. de Tecnología y Comunicación Prehospitalaria", 3, ["7028", "7029"], 4],
      ["7038", "Preparación para la Respuesta y Manejo de Desastres", 3, ["7031"], 4],
      ["4764", "Inglés 449B", 3, ["4763"], 4],
      ["7039", "Práctica Universitaria IV", 6, ["7033"], 4],
      ["5343", "Historia de Panamá", 3, [], 5],
      ["5344", "Geografía de Panamá", 3, [], 5],
      ["0529", "Español", 3, [], 5],
      ["7040", "Bioestadística", 3, [], 5],
      ["2748", "Electrocardiografía", 3, ["7034"], 6],
      ["7041", "Urgencias Gineco-Obstétricas", 3, ["7034", "7035"], 6],
      ["7042", "Clínica de Urgencias Gineco-Obstétricas", 2, ["7034", "7035"], 6],
      ["7043", "Farmacología de Urgencias I", 3, ["2721"], 6],
      ["7044", "Rescate y Supervivencia en la Selva", 4, ["7032"], 6],
      ["4765", "Inglés 450A", 2, ["4764"], 6],
      ["7045", "Práctica Universitaria V", 5, ["7039"], 6],
      ["7046", "Urgencias Pediátricas", 3, ["7041"], 7],
      ["7047", "Clínica de Urgencias Pediátricas", 2, ["7041", "7042"], 7],
      ["7048", "Manejo de Múltiples Víctimas y Triaje", 2, ["7041", "7042"], 7],
      ["7049", "Farmacología de Urgencias II", 3, ["7043"], 7],
      ["7050", "Metodología de la Investigación I", 3, ["7040"], 7],
      ["4766", "Inglés 450B", 2, ["4765"], 7],
      ["7051", "Práctica Universitaria VI", 6, ["7045", "2748"], 7],
      ["5348", "Seminario de Iniciativa Empresarial", 0, [], 7],
      ["2742", "Educación para la Atención a la Diversidad", 3, [], 8],
      ["7052", "Salud Pública", 3, [], 8],
      ["7053", "Sem. de Seguridad Ocupacional (Optativa)", 2, [], 8],
      ["SEG-SALV", "Seguridad y Salvamento Acuático", 3, ["7026"], 8],
      ["7055", "Imagenología en Emergencia", 3, ["7048"], 9],
      ["7056", "Manejo de Vías Aéreas Avanzado", 3, ["7048", "7049"], 9],
      ["7057", "Gestión de Servicios Propios", 3, ["7052"], 9],
      ["7058", "Aerotransp. Médica y Rescate Aéreo", 3, ["SEG-SALV", "7048"], 9],
      ["7059", "Recuperación en Desastres y Desarrollo Seguro", 3, ["7038"], 9],
      ["7060", "Metodología de la Investigación II", 3, ["7050"], 9],
      ["7061", "Práctica Universitaria VII", 6, ["7051", "7049"], 9],
      ["7062", "Trabajo de Grado y Práctica Profesional", 28, ["7061"], 10]
    ];

    const container = document.getElementById("container");
    const courseMap = {};

    semesters.forEach((sem, i) => {
      const section = document.createElement("section");
      const title = document.createElement("h2");
      title.textContent = sem.title;
      section.appendChild(title);

      const grid = document.createElement("div");
      grid.className = "grid";
      grid.style.backgroundColor = sem.color;
      grid.style.padding = "10px";
      grid.style.borderRadius = "8px";

      courses.filter(c => c[4] === i).forEach(([code, name, credits, prereq]) => {
        const card = document.createElement("div");
        card.className = "card";
        card.id = code;
        card.innerHTML = `<strong>${name}</strong><br><small>${code} • ${credits} créditos</small>`;
        card.onclick = () => toggleComplete(code);
        courseMap[code] = { prereq, element: card };
        grid.appendChild(card);
      });

      section.appendChild(grid);
      container.appendChild(section);
    });

    function toggleComplete(code) {
      const card = courseMap[code].element;
      card.classList.toggle("completed");
      updateHighlights();
    }

    function updateHighlights() {
      Object.entries(courseMap).forEach(([code, { prereq, element }]) => {
        if (element.classList.contains("completed")) {
          element.classList.remove("highlight");
          element.style.opacity = 0.6;
          return;
        }
        const unlocked = prereq.every(p => courseMap[p]?.element.classList.contains("completed"));
        element.classList.toggle("highlight", unlocked);
        element.style.opacity = unlocked ? 1 : 0.4;
      });
    }

    updateHighlights();
  </script>
</body>
</html>
 
