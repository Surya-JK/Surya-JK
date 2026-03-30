import { useState, useEffect, useRef } from "react";

const skills = [
  { cat: "Languages", items: ["Python", "Java", "JavaScript"], color: "#5DCAA5" },
  { cat: "Frontend", items: ["React.js", "HTML", "CSS", "Bootstrap"], color: "#7F77DD" },
  { cat: "Databases", items: ["MySQL", "SQLite", "Oracle SQL"], color: "#EF9F27" },
  { cat: "ML / AI", items: ["Computer Vision", "Data Analysis"], color: "#D85A30" },
  { cat: "Tools", items: ["Docker", "Apache Superset", "Git"], color: "#378ADD" },
  { cat: "Cloud", items: ["Oracle Cloud", "Verizon Forage"], color: "#D4537E" },
];

const projects = [
  {
    icon: "🚗",
    title: "Car Damage Assessment",
    sub: "Kaspon Techworks · Nov–Dec 2025",
    desc: "AI model using Computer Vision to detect & assess vehicle damage end-to-end.",
    tags: ["Python", "Computer Vision", "ML"],
    color: "#E6F1FB",
    accent: "#378ADD",
    link: "https://github.com/Surya-JK/Kaspon-Techworks-Intern-Project-Car-Damage-Assesment",
  },
  {
    icon: "📊",
    title: "FP Tickets Dashboard",
    sub: "Kaspon Techworks · Nov–Dec 2025",
    desc: "Interactive data visualization dashboard built with Apache Superset.",
    tags: ["Apache Superset", "SQL", "Data Viz"],
    color: "#E1F5EE",
    accent: "#1D9E75",
    link: "https://github.com/Surya-JK/Kaspon-Techworks-Intern-Project-FP-Tickets-Dashboard-using-Apache-Superset",
  },
  {
    icon: "🌐",
    title: "BonTon Chatbot & E-commerce",
    sub: "BonTon Softwares · Jul–Oct 2025",
    desc: "Full-stack chatbot, e-commerce app & prototype site with React and MySQL.",
    tags: ["React v19", "MySQL", "Bootstrap"],
    color: "#FAEEDA",
    accent: "#BA7517",
    link: "https://github.com/Surya-JK/Intern-Project-Prototype-BonTon-Website",
  },
  {
    icon: "🤖",
    title: "Virtual Assistant + Weather Monitor",
    sub: "Personal Projects · 2024",
    desc: "Voice-enabled assistant and real-time weather tracking system in Python.",
    tags: ["Python"],
    color: "#EEEDFE",
    accent: "#7F77DD",
    link: "https://github.com/Surya-JK/Virtual-Assistant",
  },
];

const certs = [
  { name: "Oracle SQL Specialist", issuer: "Oracle", score: "86%" },
  { name: "Data Science Foundations", issuer: "IBM", score: null },
  { name: "Gemini Certified", issuer: "Google", score: null },
  { name: "Python + Java", issuer: "HackerRank", score: null },
  { name: "Ethical Hacking", issuer: "NPTEL", score: null },
  { name: "Cyber Security", issuer: "NPTEL", score: null },
  { name: "Cloud Simulation", issuer: "Verizon · Forage", score: null },
  { name: "Hackforge'25", issuer: "SRM Vadapalani", score: null },
];

const roles = ["ML Engineer", "Full-stack Dev", "Data Analyst", "AI Enthusiast"];

function TypeWriter({ words }) {
  const [display, setDisplay] = useState("");
  const [wIdx, setWIdx] = useState(0);
  const [cIdx, setCIdx] = useState(0);
  const [deleting, setDeleting] = useState(false);
  useEffect(() => {
    const word = words[wIdx];
    const speed = deleting ? 50 : 90;
    const timer = setTimeout(() => {
      if (!deleting && cIdx < word.length) {
        setDisplay(word.slice(0, cIdx + 1));
        setCIdx(cIdx + 1);
      } else if (!deleting && cIdx === word.length) {
        setTimeout(() => setDeleting(true), 1200);
      } else if (deleting && cIdx > 0) {
        setDisplay(word.slice(0, cIdx - 1));
        setCIdx(cIdx - 1);
      } else {
        setDeleting(false);
        setWIdx((wIdx + 1) % words.length);
      }
    }, speed);
    return () => clearTimeout(timer);
  }, [cIdx, deleting, wIdx, words]);
  return (
    <span style={{ color: "#5DCAA5", fontFamily: "'Courier New', monospace" }}>
      {display}
      <span style={{ animation: "blink 1s step-end infinite", color: "#5DCAA5" }}>|</span>
    </span>
  );
}

function AnimatedBar({ value, color, delay }) {
  const [width, setWidth] = useState(0);
  useEffect(() => {
    const t = setTimeout(() => setWidth(value), delay);
    return () => clearTimeout(t);
  }, [value, delay]);
  return (
    <div style={{ background: "#1a1a2e", borderRadius: 4, height: 6, overflow: "hidden" }}>
      <div style={{ width: `${width}%`, height: "100%", background: color, borderRadius: 4, transition: "width 1.2s cubic-bezier(0.4,0,0.2,1)" }} />
    </div>
  );
}

function ParticleCanvas() {
  const ref = useRef();
  useEffect(() => {
    const canvas = ref.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
    const particles = Array.from({ length: 40 }, () => ({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      r: Math.random() * 1.5 + 0.5,
      dx: (Math.random() - 0.5) * 0.3,
      dy: (Math.random() - 0.5) * 0.3,
      alpha: Math.random() * 0.5 + 0.2,
    }));
    let raf;
    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      particles.forEach(p => {
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(93,202,165,${p.alpha})`;
        ctx.fill();
        p.x += p.dx;
        p.y += p.dy;
        if (p.x < 0 || p.x > canvas.width) p.dx *= -1;
        if (p.y < 0 || p.y > canvas.height) p.dy *= -1;
      });
      raf = requestAnimationFrame(draw);
    }
    draw();
    return () => cancelAnimationFrame(raf);
  }, []);
  return <canvas ref={ref} style={{ position: "absolute", inset: 0, width: "100%", height: "100%", pointerEvents: "none" }} />;
}

function FadeIn({ children, delay = 0, y = 20 }) {
  const [visible, setVisible] = useState(false);
  useEffect(() => {
    const t = setTimeout(() => setVisible(true), delay);
    return () => clearTimeout(t);
  }, [delay]);
  return (
    <div style={{ opacity: visible ? 1 : 0, transform: visible ? "translateY(0)" : `translateY(${y}px)`, transition: "opacity 0.6s ease, transform 0.6s ease" }}>
      {children}
    </div>
  );
}

export default function App() {
  const [hoveredProj, setHoveredProj] = useState(null);
  const [copied, setCopied] = useState(false);

  const readme = `<div align="center">

# Hey, I'm Surya JK 👋

**AI & Data Science · ML Engineer · Full-stack Developer · Data Analyst**

> *"I build things that think — from computer vision models to full-stack apps and data dashboards."*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-suryajk-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/suryajk)
[![Portfolio](https://img.shields.io/badge/Portfolio-surya--jk-orange?style=flat-square&logo=firefox&logoColor=white)](https://surya-jk-portfolio.onrender.com)
[![Email](https://img.shields.io/badge/Email-jksurya06@gmail.com-D44638?style=flat-square&logo=gmail&logoColor=white)](mailto:jksurya06@gmail.com)
[![Views](https://komarev.com/ghpvc/?username=Surya-JK&label=Profile+Views&color=1D9E75&style=flat-square)](https://github.com/Surya-JK)
[![CGPA](https://img.shields.io/badge/CGPA-9.08%20%2F%2010-5DCAA5?style=flat-square)](https://github.com/Surya-JK)

</div>

---

## 🧠 About me

B.Tech in **Artificial Intelligence & Data Science** @ Saveetha School of Engineering, Chennai.
I love turning raw data and messy problems into real solutions — CV models, full-stack apps, dashboards, you name it.

- 🔭 Currently: AI/ML projects + full-stack development
- 🌱 Learning: latest in ML, cloud & system design
- 🤝 Open to: internships, collabs, interesting problems
- 🏏 Outside code: cricket & good music

---

## 🛠 Tech Stack

\`\`\`
Languages  →  Python · Java · JavaScript
Frontend   →  React.js · HTML · CSS · Bootstrap
Databases  →  MySQL · SQLite · Oracle SQL
ML / AI    →  Computer Vision · Data Analysis
Tools      →  Docker · Apache Superset · Git
Cloud      →  Oracle Cloud · Verizon (Forage)
\`\`\`

---

## 🚀 Projects

### 🚗 [Car Damage Assessment](https://github.com/Surya-JK/Kaspon-Techworks-Intern-Project-Car-Damage-Assesment)
> Kaspon Techworks Internship · Nov–Dec 2025

End-to-end AI model using **Computer Vision** to detect & assess vehicle damage from images.
\`Python\` \`Computer Vision\` \`ML\`

---

### 📊 [FP Tickets Dashboard](https://github.com/Surya-JK/Kaspon-Techworks-Intern-Project-FP-Tickets-Dashboard-using-Apache-Superset)
> Kaspon Techworks Internship · Nov–Dec 2025

Interactive data visualization dashboard for field performance tickets.
\`Apache Superset\` \`SQL\` \`Data Visualization\`

---

### 🌐 [BonTon Chatbot & E-commerce App](https://github.com/Surya-JK/Intern-Project-Prototype-BonTon-Website)
> BonTon Softwares Internship · Jul–Oct 2025

Full-stack chatbot, e-commerce app, and company prototype covering frontend + backend.
\`React v19\` \`MySQL\` \`JavaScript\` \`Bootstrap\`

---

### 🤖 [Virtual Assistant](https://github.com/Surya-JK/Virtual-Assistant) · 🌦 [Weather Monitor](https://github.com/Surya-JK/Weather-Monitoring-System)
Personal Python projects — voice assistant and real-time weather tracker.
\`Python\`

---

## 🏅 Certifications

| Certification | Issuer | Score |
|---|---|---|
| Oracle SQL Specialist | Oracle | 86% |
| Data Science Foundations L1 | IBM | — |
| Gemini Certified Student | Google | — |
| Python & Java | HackerRank | — |
| Ethical Hacking | NPTEL | — |
| Cyber Security & Privacy | NPTEL | — |
| Cloud Platform Simulation | Verizon · Forage | — |

---

## 📊 GitHub Activity

[![Activity Graph](https://github-readme-activity-graph.vercel.app/graph?username=Surya-JK&theme=react-dark&hide_border=true&area=true)](https://github.com/Surya-JK)

[![Profile Views](https://komarev.com/ghpvc/?username=Surya-JK&color=1D9E75&style=flat-square&label=Profile+Views)](https://github.com/Surya-JK)

---

<div align="center">
  <i>Always open to meaningful conversations, collabs, and a good cricket debate.</i><br><br>
  <b>Let's build something →</b> <a href="mailto:jksurya06@gmail.com">jksurya06@gmail.com</a>
</div>`;

  const copy = () => {
    navigator.clipboard.writeText(readme).then(() => {
      setCopied(true);
      setTimeout(() => setCopied(false), 2500);
    });
  };

  return (
    <div style={{ background: "#0d0d1a", minHeight: "100vh", fontFamily: "'Segoe UI', sans-serif", color: "#e0e0f0", padding: "0 0 40px" }}>
      <style>{`
        @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
        @keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-6px)} }
        @keyframes spin { to{transform:rotate(360deg)} }
        @keyframes pulse { 0%,100%{opacity:0.6} 50%{opacity:1} }
        @keyframes shimmer { 0%{background-position:-200% 0} 100%{background-position:200% 0} }
        .proj-card { transition: transform 0.3s ease, box-shadow 0.3s ease; }
        .proj-card:hover { transform: translateY(-4px); }
        .skill-pill:hover { transform: scale(1.05); transition: transform 0.2s; }
        .cert-item:hover { border-color: #5DCAA5 !important; transition: border-color 0.2s; }
        .copy-btn:hover { background: #1D9E75 !important; }
        ::-webkit-scrollbar { width: 4px; } 
        ::-webkit-scrollbar-track { background: #0d0d1a; }
        ::-webkit-scrollbar-thumb { background: #2a2a4a; border-radius: 4px; }
      `}</style>

      {/* Hero */}
      <div style={{ position: "relative", background: "linear-gradient(135deg, #0d0d1a 0%, #111128 50%, #0d1a0d 100%)", borderBottom: "1px solid #1a2a1a", padding: "48px 32px 40px", overflow: "hidden" }}>
        <ParticleCanvas />
        <div style={{ position: "relative", zIndex: 1, maxWidth: 760, margin: "0 auto" }}>
          <FadeIn delay={0}>
            <div style={{ display: "flex", alignItems: "center", gap: 20, marginBottom: 20 }}>
              <div style={{ width: 64, height: 64, borderRadius: "50%", background: "linear-gradient(135deg, #5DCAA5, #378ADD)", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 22, fontWeight: 700, color: "#fff", animation: "float 3s ease-in-out infinite", flexShrink: 0, border: "2px solid #1D9E75" }}>
                SJK
              </div>
              <div>
                <div style={{ fontSize: 28, fontWeight: 700, letterSpacing: -0.5, color: "#fff" }}>Surya JK</div>
                <div style={{ fontSize: 13, color: "#888", marginTop: 2 }}>Chennai, India · B.Tech AI & DS @ Saveetha</div>
              </div>
            </div>
          </FadeIn>

          <FadeIn delay={150}>
            <div style={{ fontSize: 17, color: "#b0b0d0", marginBottom: 16, lineHeight: 1.5 }}>
              I'm a{" "}<TypeWriter words={roles} />
            </div>
          </FadeIn>

          <FadeIn delay={250}>
            <div style={{ fontSize: 13, color: "#667", fontStyle: "italic", borderLeft: "2px solid #5DCAA5", paddingLeft: 12, marginBottom: 20, color: "#7a9a8a" }}>
              "I build things that think — from CV models to full-stack apps and data dashboards. Always learning, always shipping."
            </div>
          </FadeIn>

          <FadeIn delay={350}>
            <div style={{ display: "flex", flexWrap: "wrap", gap: 8 }}>
              {[
                { label: "ML Engineer", c: "#5DCAA5", bg: "#0a2a1f" },
                { label: "Full-stack Dev", c: "#7F77DD", bg: "#1a1830" },
                { label: "Data Analyst", c: "#EF9F27", bg: "#2a1f00" },
                { label: "CGPA 9.08", c: "#D85A30", bg: "#2a1008" },
                { label: "Open to work", c: "#D4537E", bg: "#2a0a18" },
              ].map(b => (
                <span key={b.label} className="skill-pill" style={{ background: b.bg, color: b.c, border: `1px solid ${b.c}40`, borderRadius: 20, padding: "4px 12px", fontSize: 12, fontWeight: 500, cursor: "default" }}>{b.label}</span>
              ))}
            </div>
          </FadeIn>
        </div>
      </div>

      <div style={{ maxWidth: 760, margin: "0 auto", padding: "0 24px" }}>

        {/* Skills */}
        <FadeIn delay={400}>
          <div style={{ marginTop: 32 }}>
            <div style={{ fontSize: 11, color: "#5DCAA5", letterSpacing: "0.1em", textTransform: "uppercase", fontWeight: 600, marginBottom: 14 }}>// Tech Stack</div>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(3, 1fr)", gap: 10 }}>
              {skills.map((s, i) => (
                <FadeIn key={s.cat} delay={450 + i * 60}>
                  <div style={{ background: "#111128", border: "1px solid #1e1e3a", borderRadius: 10, padding: "12px 14px" }}>
                    <div style={{ fontSize: 10, color: s.color, textTransform: "uppercase", letterSpacing: "0.08em", marginBottom: 6, fontWeight: 600 }}>{s.cat}</div>
                    <div style={{ fontSize: 12, color: "#c0c0e0", lineHeight: 1.6 }}>{s.items.join(" · ")}</div>
                    <AnimatedBar value={70 + i * 5} color={s.color} delay={600 + i * 80} />
                  </div>
                </FadeIn>
              ))}
            </div>
          </div>
        </FadeIn>

        {/* Projects */}
        <FadeIn delay={700}>
          <div style={{ marginTop: 36 }}>
            <div style={{ fontSize: 11, color: "#5DCAA5", letterSpacing: "0.1em", textTransform: "uppercase", fontWeight: 600, marginBottom: 14 }}>// Projects</div>
            <div style={{ display: "flex", flexDirection: "column", gap: 12 }}>
              {projects.map((p, i) => (
                <FadeIn key={p.title} delay={750 + i * 80}>
                  <div className="proj-card" onMouseEnter={() => setHoveredProj(i)} onMouseLeave={() => setHoveredProj(null)}
                    style={{ background: "#111128", border: `1px solid ${hoveredProj === i ? p.accent + "80" : "#1e1e3a"}`, borderRadius: 12, padding: "16px 18px", display: "flex", gap: 14, cursor: "pointer", boxShadow: hoveredProj === i ? `0 8px 32px ${p.accent}20` : "none" }}
                    onClick={() => window.open(p.link, "_blank")}>
                    <div style={{ width: 40, height: 40, borderRadius: 10, background: p.color + "20", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 18, flexShrink: 0, border: `1px solid ${p.accent}30` }}>{p.icon}</div>
                    <div style={{ flex: 1 }}>
                      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
                        <div style={{ fontSize: 14, fontWeight: 600, color: "#e0e0f8" }}>{p.title}</div>
                        <div style={{ fontSize: 10, color: p.accent, opacity: hoveredProj === i ? 1 : 0, transition: "opacity 0.2s" }}>↗ view</div>
                      </div>
                      <div style={{ fontSize: 11, color: "#555", marginTop: 2, marginBottom: 6 }}>{p.sub}</div>
                      <div style={{ fontSize: 12, color: "#8888aa", lineHeight: 1.5 }}>{p.desc}</div>
                      <div style={{ display: "flex", flexWrap: "wrap", gap: 5, marginTop: 8 }}>
                        {p.tags.map(t => (
                          <span key={t} style={{ fontSize: 10, padding: "2px 8px", background: p.accent + "18", color: p.accent, border: `1px solid ${p.accent}40`, borderRadius: 6 }}>{t}</span>
                        ))}
                      </div>
                    </div>
                  </div>
                </FadeIn>
              ))}
            </div>
          </div>
        </FadeIn>

        {/* Certs */}
        <FadeIn delay={1000}>
          <div style={{ marginTop: 36 }}>
            <div style={{ fontSize: 11, color: "#5DCAA5", letterSpacing: "0.1em", textTransform: "uppercase", fontWeight: 600, marginBottom: 14 }}>// Certifications</div>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 8 }}>
              {certs.map((c, i) => (
                <div key={c.name} className="cert-item" style={{ background: "#111128", border: "1px solid #1e1e3a", borderRadius: 8, padding: "10px 12px", display: "flex", justifyContent: "space-between", alignItems: "center" }}>
                  <div>
                    <div style={{ fontSize: 12, fontWeight: 500, color: "#d0d0f0" }}>{c.name}</div>
                    <div style={{ fontSize: 11, color: "#555", marginTop: 2 }}>{c.issuer}</div>
                  </div>
                  {c.score && <span style={{ fontSize: 11, padding: "2px 8px", background: "#0a2a1f", color: "#5DCAA5", border: "1px solid #5DCAA530", borderRadius: 6, fontWeight: 600 }}>{c.score}</span>}
                </div>
              ))}
            </div>
          </div>
        </FadeIn>

        {/* Fun + Connect */}
        <FadeIn delay={1100}>
          <div style={{ marginTop: 36, display: "grid", gridTemplateColumns: "1fr 1fr", gap: 16 }}>
            <div>
              <div style={{ fontSize: 11, color: "#5DCAA5", letterSpacing: "0.1em", textTransform: "uppercase", fontWeight: 600, marginBottom: 12 }}>// Beyond code</div>
              <div style={{ display: "flex", flexDirection: "column", gap: 8 }}>
                {[{ e: "🏏", l: "Cricket" }, { e: "🎵", l: "Music" }, { e: "📖", l: "Always learning" }, { e: "💬", l: "Tamil & English" }].map(f => (
                  <div key={f.l} style={{ display: "flex", alignItems: "center", gap: 10, fontSize: 13, color: "#8888aa" }}>
                    <span style={{ fontSize: 16 }}>{f.e}</span>{f.l}
                  </div>
                ))}
              </div>
            </div>
            <div>
              <div style={{ fontSize: 11, color: "#5DCAA5", letterSpacing: "0.1em", textTransform: "uppercase", fontWeight: 600, marginBottom: 12 }}>// Connect</div>
              <div style={{ display: "flex", flexDirection: "column", gap: 8 }}>
                {[
                  { label: "LinkedIn", url: "https://linkedin.com/in/suryajk" },
                  { label: "Portfolio", url: "https://surya-jk-portfolio.onrender.com" },
                  { label: "Email", url: "mailto:jksurya06@gmail.com" },
                ].map(l => (
                  <a key={l.label} href={l.url} style={{ fontSize: 13, color: "#5DCAA5", textDecoration: "none", display: "flex", alignItems: "center", gap: 6 }}>
                    <span style={{ fontSize: 10, opacity: 0.6 }}>↗</span>{l.label}
                  </a>
                ))}
              </div>
            </div>
          </div>
        </FadeIn>

        {/* Copy README */}
        <FadeIn delay={1200}>
          <div style={{ marginTop: 36, background: "#111128", border: "1px solid #1e1e3a", borderRadius: 12, overflow: "hidden" }}>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: "12px 16px", borderBottom: "1px solid #1e1e3a", background: "#0d0d1a" }}>
              <div style={{ display: "flex", gap: 6 }}>
                <div style={{ width: 10, height: 10, borderRadius: "50%", background: "#D85A30" }} />
                <div style={{ width: 10, height: 10, borderRadius: "50%", background: "#EF9F27" }} />
                <div style={{ width: 10, height: 10, borderRadius: "50%", background: "#5DCAA5" }} />
                <span style={{ fontSize: 12, color: "#555", marginLeft: 8 }}>README.md</span>
              </div>
              <button className="copy-btn" onClick={copy}
                style={{ padding: "6px 16px", background: copied ? "#1D9E75" : "#1a1a2e", color: copied ? "#fff" : "#5DCAA5", border: "1px solid #5DCAA5", borderRadius: 6, fontSize: 12, fontWeight: 600, cursor: "pointer", transition: "all 0.3s" }}>
                {copied ? "✓ Copied!" : "Copy README"}
              </button>
            </div>
            <div style={{ padding: 16, fontFamily: "monospace", fontSize: 11, color: "#555", lineHeight: 1.8, maxHeight: 140, overflow: "auto" }}>
              {readme.slice(0, 600)}...<br /><span style={{ color: "#5DCAA5" }}># click "Copy README" to get the full markdown</span>
            </div>
          </div>
        </FadeIn>

        <FadeIn delay={1300}>
          <div style={{ marginTop: 32, textAlign: "center", fontSize: 12, color: "#333", borderTop: "1px solid #1a1a2e", paddingTop: 20 }}>
            Always open to meaningful conversations, collabs, and a good cricket debate 🏏
          </div>
        </FadeIn>
      </div>
    </div>
  );
}
