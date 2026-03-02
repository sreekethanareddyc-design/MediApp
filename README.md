import { useState, useEffect, useRef } from "react";

// ─── BANGALORE HOSPITALS ─────────────────────────────────────────────────────
const HOSPITALS = {
  Cardiology: [
    { id: 1, name: "Narayana Health City", area: "Bommasandra", rating: 4.9, reviews: 5820, wait: "~20 min", beds: 3000, type: "Private", emergency: true, phone: "080-71222222", address: "258/A, Bommasandra Industrial Area", img: "🏥" },
    { id: 2, name: "Manipal Hospital", area: "HAL Airport Road", rating: 4.8, reviews: 4310, wait: "~25 min", beds: 650, type: "Private", emergency: true, phone: "080-25023456", address: "98, HAL Airport Road", img: "🏨" },
    { id: 3, name: "Fortis Hospital", area: "Bannerghatta Road", rating: 4.7, reviews: 3890, wait: "~30 min", beds: 400, type: "Private", emergency: true, phone: "080-66214444", address: "154/9, Bannerghatta Road", img: "🏦" },
  ],
  Neurology: [
    { id: 4, name: "NIMHANS", area: "Hosur Road", rating: 4.9, reviews: 6700, wait: "~45 min", beds: 900, type: "Government", emergency: true, phone: "080-46110007", address: "Hosur Road, Lakkasandra", img: "🧠" },
    { id: 5, name: "Apollo Hospital", area: "Bannerghatta Road", rating: 4.8, reviews: 3200, wait: "~20 min", beds: 350, type: "Private", emergency: true, phone: "080-26304050", address: "154/11, Bannerghatta Rd", img: "🏥" },
    { id: 6, name: "BGS Gleneagles Global", area: "Kengeri", rating: 4.6, reviews: 2100, wait: "~25 min", beds: 500, type: "Private", emergency: true, phone: "080-26730000", address: "67, Uttarahalli Road, Kengeri", img: "🏨" },
  ],
  Orthopedics: [
    { id: 7, name: "St. John's Medical College", area: "Koramangala", rating: 4.8, reviews: 4500, wait: "~30 min", beds: 1240, type: "Private", emergency: true, phone: "080-22065000", address: "Sarjapur Road, Koramangala", img: "🏥" },
    { id: 8, name: "Sakra World Hospital", area: "Marathahalli", rating: 4.7, reviews: 2900, wait: "~20 min", beds: 350, type: "Private", emergency: true, phone: "080-49690000", address: "52/2, Devarabeesanahalli, Marathahalli", img: "🏦" },
    { id: 9, name: "Aster CMI Hospital", area: "Hebbal", rating: 4.7, reviews: 3100, wait: "~25 min", beds: 430, type: "Private", emergency: true, phone: "080-43422222", address: "43/2, New Airport Road, Hebbal", img: "🏨" },
  ],
  Dermatology: [
    { id: 10, name: "Cutis Academy", area: "Rajajinagar", rating: 4.6, reviews: 1800, wait: "~15 min", beds: 50, type: "Private", emergency: false, phone: "080-23322299", address: "No. 1, 8th Main, Rajajinagar", img: "🏥" },
    { id: 11, name: "Manipal Hospital", area: "Whitefield", rating: 4.7, reviews: 2200, wait: "~20 min", beds: 280, type: "Private", emergency: true, phone: "080-22081111", address: "ITPL Main Rd, Whitefield", img: "🏨" },
    { id: 12, name: "Vikram Hospital", area: "Millers Road", rating: 4.5, reviews: 1400, wait: "~20 min", beds: 150, type: "Private", emergency: true, phone: "080-40206000", address: "71/1, Millers Road, Vasanth Nagar", img: "🏦" },
  ],
  Gastroenterology: [
    { id: 13, name: "Aster CMI Hospital", area: "Hebbal", rating: 4.8, reviews: 3100, wait: "~25 min", beds: 430, type: "Private", emergency: true, phone: "080-43422222", address: "43/2, New Airport Road, Hebbal", img: "🏥" },
    { id: 14, name: "Columbia Asia Hospital", area: "Yeshwanthpur", rating: 4.6, reviews: 2400, wait: "~20 min", beds: 180, type: "Private", emergency: true, phone: "080-61222222", address: "26/4, Brigade Gateway, Yeshwanthpur", img: "🏨" },
    { id: 15, name: "Cloudnine Hospital", area: "Jayanagar", rating: 4.7, reviews: 1900, wait: "~15 min", beds: 120, type: "Private", emergency: false, phone: "080-69004000", address: "1533, 9th Main, Jayanagar", img: "🏦" },
  ],
  ENT: [
    { id: 16, name: "KIMS Hospital", area: "Banashankari", rating: 4.6, reviews: 2100, wait: "~20 min", beds: 250, type: "Private", emergency: true, phone: "080-26762626", address: "HK Complex, Banashankari", img: "🏥" },
    { id: 17, name: "Sparsh Hospital", area: "Infantry Road", rating: 4.7, reviews: 1700, wait: "~15 min", beds: 100, type: "Private", emergency: true, phone: "080-40505050", address: "29/P-2, NT Road, Infantry Road", img: "🏨" },
  ],
  Pulmonology: [
    { id: 18, name: "Narayana Health City", area: "Bommasandra", rating: 4.9, reviews: 5820, wait: "~20 min", beds: 3000, type: "Private", emergency: true, phone: "080-71222222", address: "258/A, Bommasandra Industrial Area", img: "🏥" },
    { id: 19, name: "Fortis Hospital", area: "Cunningham Road", rating: 4.7, reviews: 2800, wait: "~30 min", beds: 250, type: "Private", emergency: true, phone: "080-66214444", address: "14, Cunningham Road", img: "🏦" },
  ],
  Pediatrics: [
    { id: 20, name: "Indira Gandhi Children's Hospital", area: "Shivajinagar", rating: 4.5, reviews: 3400, wait: "~40 min", beds: 350, type: "Government", emergency: true, phone: "080-22867800", address: "Miller's Road, Shivajinagar", img: "🏥" },
    { id: 21, name: "Rainbow Children's Hospital", area: "Marathahalli", rating: 4.8, reviews: 2900, wait: "~20 min", beds: 200, type: "Private", emergency: true, phone: "080-68181818", address: "78, Chikka Bellandur, Marathahalli", img: "🏨" },
    { id: 22, name: "Cloudnine Hospital", area: "Bellandur", rating: 4.8, reviews: 2600, wait: "~15 min", beds: 150, type: "Private", emergency: true, phone: "080-69004000", address: "Bellandur Gate, Sarjapur Road", img: "🏦" },
  ],
  General: [
    { id: 23, name: "Victoria Hospital", area: "City Market", rating: 4.3, reviews: 6100, wait: "~60 min", beds: 1300, type: "Government", emergency: true, phone: "080-26706000", address: "Fort Road, City Market", img: "🏥" },
    { id: 24, name: "Bowring & Lady Curzon Hospital", area: "Shivajinagar", rating: 4.2, reviews: 4200, wait: "~50 min", beds: 960, type: "Government", emergency: true, phone: "080-25591904", address: "Shivajinagar", img: "🏨" },
    { id: 25, name: "Columbia Asia Hospital", area: "Whitefield", rating: 4.7, reviews: 2200, wait: "~20 min", beds: 180, type: "Private", emergency: true, phone: "080-61222222", address: "ITPL Road, Whitefield", img: "🏦" },
  ],
  Gynecology: [
    { id: 26, name: "Cloudnine Hospital", area: "Old Airport Road", rating: 4.9, reviews: 3800, wait: "~15 min", beds: 120, type: "Private", emergency: true, phone: "080-69004000", address: "1533, Old Airport Road", img: "🏥" },
    { id: 27, name: "St. Martha's Hospital", area: "Nrupatunga Road", rating: 4.6, reviews: 2900, wait: "~30 min", beds: 450, type: "Private", emergency: true, phone: "080-22270423", address: "Nrupatunga Road", img: "🏨" },
  ],
  Psychiatry: [
    { id: 28, name: "NIMHANS", area: "Hosur Road", rating: 4.9, reviews: 6700, wait: "~45 min", beds: 900, type: "Government", emergency: true, phone: "080-46110007", address: "Hosur Road, Lakkasandra", img: "🧠" },
    { id: 29, name: "Vandrevala Foundation", area: "Indiranagar", rating: 4.7, reviews: 1200, wait: "~20 min", beds: 60, type: "Private", emergency: false, phone: "1860-2662-345", address: "100 Feet Road, Indiranagar", img: "🏥" },
  ],
};

// ─── SYMPTOM FLOW QUESTIONS ───────────────────────────────────────────────────
const QUESTIONS = [
  {
    id: "main_symptom",
    text: "What is the **main symptom** you are experiencing right now?",
    type: "quick",
    options: ["Chest pain / palpitations", "Headache / dizziness", "Fever / chills", "Cough / breathing difficulty", "Stomach / abdominal pain", "Skin rash / irritation", "Joint / bone / back pain", "Ear / nose / throat issue", "Child health concern", "Mental health / anxiety", "Women's health concern", "Other"],
    key: "mainSymptom"
  },
  {
    id: "duration",
    text: "How **long** have you been experiencing this?",
    type: "quick",
    options: ["Just started today", "1–2 days", "3–5 days", "1–2 weeks", "More than 2 weeks", "It comes and goes"],
    key: "duration"
  },
  {
    id: "severity",
    text: "On a scale of 1–10, how would you rate the **severity** of your discomfort?",
    type: "scale",
    key: "severity"
  },
  {
    id: "additional",
    text: "Are you experiencing any of these **additional symptoms**? (Select all that apply)",
    type: "multi",
    options: ["Nausea / vomiting", "High fever (>102°F)", "Difficulty breathing", "Extreme fatigue", "Loss of appetite", "Swelling", "Numbness / tingling", "None of these"],
    key: "additional"
  },
  {
    id: "age_group",
    text: "What is your **age group**?",
    type: "quick",
    options: ["Child (0–12)", "Teenager (13–17)", "Adult (18–40)", "Middle-aged (41–60)", "Senior (60+)"],
    key: "ageGroup"
  },
  {
    id: "medical_history",
    text: "Do you have any **pre-existing conditions** or allergies we should know about?",
    type: "multi",
    options: ["Diabetes", "Hypertension", "Heart disease", "Asthma / lung issues", "No known conditions", "Prefer not to say"],
    key: "history"
  },
  {
    id: "hospital_pref",
    text: "Do you have a **hospital preference**?",
    type: "quick",
    options: ["Nearest available", "Government hospital (affordable)", "Private hospital (premium)", "No preference"],
    key: "hospPref"
  },
];

// ─── SPECIALTY MAP ────────────────────────────────────────────────────────────
const mapSpecialty = (symptom, ageGroup) => {
  if (ageGroup === "Child (0–12)" || ageGroup === "Teenager (13–17)") return "Pediatrics";
  const map = {
    "Chest pain / palpitations": "Cardiology",
    "Headache / dizziness": "Neurology",
    "Fever / chills": "General",
    "Cough / breathing difficulty": "Pulmonology",
    "Stomach / abdominal pain": "Gastroenterology",
    "Skin rash / irritation": "Dermatology",
    "Joint / bone / back pain": "Orthopedics",
    "Ear / nose / throat issue": "ENT",
    "Child health concern": "Pediatrics",
    "Mental health / anxiety": "Psychiatry",
    "Women's health concern": "Gynecology",
    "Other": "General",
  };
  return map[symptom] || "General";
};

const triageLevel = (severity, duration, additional) => {
  const sev = parseInt(severity);
  const hasBreathing = additional?.includes("Difficulty breathing");
  const hasFever = additional?.includes("High fever (>102°F)");
  if (sev >= 9 || hasBreathing) return "EMERGENCY";
  if (sev >= 7 || (hasFever && duration !== "Just started today")) return "URGENT";
  if (sev >= 4 || duration === "More than 2 weeks") return "MODERATE";
  return "MILD";
};

const TRIAGE_META = {
  EMERGENCY: { color: "#FF3B30", bg: "rgba(255,59,48,0.10)", icon: "🚨", label: "Emergency — Seek care immediately" },
  URGENT:    { color: "#FF9500", bg: "rgba(255,149,0,0.10)",  icon: "⚠️", label: "Urgent — Visit within 24 hours" },
  MODERATE:  { color: "#F59E0B", bg: "rgba(245,158,11,0.10)", icon: "🔔", label: "Moderate — Consult within 2–3 days" },
  MILD:      { color: "#10B981", bg: "rgba(16,185,129,0.10)", icon: "✅", label: "Mild — Home care + monitoring" },
};

const TIMESLOTS = ["9:00 AM","9:30 AM","10:00 AM","10:30 AM","11:00 AM","2:00 PM","2:30 PM","3:00 PM","3:30 PM","4:00 PM","4:30 PM","5:00 PM"];

// ─── HELPERS ─────────────────────────────────────────────────────────────────
function fmt(text) {
  return text.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>').replace(/\n/g, '<br/>');
}

function Stars({ rating }) {
  return (
    <span>
      {[1,2,3,4,5].map(i => (
        <span key={i} style={{ color: i <= Math.round(rating) ? "#F59E0B" : "#D1D5DB", fontSize: "11px" }}>★</span>
      ))}
      <span style={{ fontSize: "11px", color: "#6B7280", marginLeft: "3px" }}>{rating}</span>
    </span>
  );
}

function TriagePill({ level }) {
  const m = TRIAGE_META[level];
  return (
    <span style={{ display:"inline-flex", alignItems:"center", gap:"4px", padding:"3px 10px", borderRadius:"20px", fontSize:"11px", fontWeight:"800", background: m.bg, color: m.color, border:`1px solid ${m.color}44` }}>
      {m.icon} {level}
    </span>
  );
}

// ─── HOSPITAL CARD ────────────────────────────────────────────────────────────
function HospCard({ h, selected, onSelect }) {
  return (
    <div onClick={() => onSelect(h)} style={{
      borderRadius:"14px", padding:"14px", cursor:"pointer", marginBottom:"8px",
      border:`1.5px solid ${selected ? "#6366F1" : "rgba(0,0,0,0.07)"}`,
      background: selected ? "rgba(99,102,241,0.06)" : "#fff",
      boxShadow: selected ? "0 4px 20px rgba(99,102,241,0.15)" : "0 2px 8px rgba(0,0,0,0.05)",
      transition:"all 0.2s"
    }}>
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start" }}>
        <div style={{ display:"flex", gap:"10px", alignItems:"center" }}>
          <div style={{ width:"40px", height:"40px", borderRadius:"12px", fontSize:"20px", background:"#EEF2FF", display:"flex", alignItems:"center", justifyContent:"center", flexShrink:0 }}>{h.img}</div>
          <div>
            <div style={{ fontWeight:"800", fontSize:"14px", color:"#111827" }}>{h.name}</div>
            <div style={{ fontSize:"11px", color:"#9CA3AF", marginTop:"1px" }}>📍 {h.area}, Bangalore</div>
            <div style={{ marginTop:"4px" }}><Stars rating={h.rating} /><span style={{ fontSize:"11px", color:"#9CA3AF", marginLeft:"4px" }}>({h.reviews.toLocaleString()} reviews)</span></div>
          </div>
        </div>
        {selected && <span style={{ color:"#6366F1", fontSize:"18px", flexShrink:0 }}>✓</span>}
      </div>
      <div style={{ display:"flex", gap:"6px", marginTop:"10px", flexWrap:"wrap" }}>
        {[
          ["⏱", h.wait],
          ["🛏", `${h.beds} beds`],
          ["🏛", h.type, h.type === "Government" ? "#10B981" : "#6366F1"],
          h.emergency ? ["🚨", "Emergency", "#FF3B30"] : null,
        ].filter(Boolean).map(([icon, text, color="#6B7280"], i) => (
          <span key={i} style={{ padding:"3px 9px", borderRadius:"8px", fontSize:"11px", fontWeight:"600", background:"#F3F4F6", color }}>
            {icon} {text}
          </span>
        ))}
      </div>
      <div style={{ fontSize:"11px", color:"#9CA3AF", marginTop:"8px" }}>📞 {h.phone}</div>
    </div>
  );
}

// ─── MAIN APP ─────────────────────────────────────────────────────────────────
export default function App() {
  const [screen, setScreen]           = useState("home");
  const [messages, setMessages]       = useState([]);
  const [qIndex, setQIndex]           = useState(0);
  const [answers, setAnswers]         = useState({});
  const [multiSel, setMultiSel]       = useState([]);
  const [scaleSel, setScaleSel]       = useState(null);
  const [typing, setTyping]           = useState(false);
  const [phase, setPhase]             = useState("intake"); // intake | results | slots | done
  const [hospitals, setHospitals]     = useState([]);
  const [specialty, setSpecialty]     = useState("");
  const [triage, setTriage]           = useState("");
  const [selectedHosp, setSelectedHosp] = useState(null);
  const [selectedDate, setSelectedDate] = useState(null);
  const [selectedSlot, setSelectedSlot] = useState(null);
  const [booking, setBooking]         = useState(null);
  const chatEndRef = useRef(null);

  useEffect(() => { chatEndRef.current?.scrollIntoView({ behavior:"smooth" }); }, [messages, typing, phase]);

  const today = new Date();
  const dates = Array.from({ length:7 }, (_,i) => {
    const d = new Date(today); d.setDate(today.getDate() + i + 1);
    return { label: d.toLocaleDateString("en-US",{weekday:"short",month:"short",day:"numeric"}), val: d.toISOString().split("T")[0] };
  });

  const startChat = () => {
    setScreen("chat");
    setMessages([]);
    setQIndex(0);
    setAnswers({});
    setPhase("intake");
    setSelectedHosp(null);
    setSelectedDate(null);
    setSelectedSlot(null);
    setBooking(null);
    setMultiSel([]);
    setScaleSel(null);
    setTimeout(() => {
      addBot({ type:"text", text:"Hello! 👋 I'm **MediBot**, your AI health assistant.\n\nI'll ask you a few quick questions to understand your symptoms and find the **best hospitals in Bangalore** for you.\n\nLet's get started!" });
      setTimeout(() => askQ(0), 900);
    }, 400);
  };

  const addBot = (msg) => setMessages(prev => [...prev, { role:"bot", ...msg }]);
  const addUser = (text) => setMessages(prev => [...prev, { role:"user", type:"text", text }]);

  const askQ = (idx) => {
    if (idx >= QUESTIONS.length) return;
    setTyping(true);
    setTimeout(() => {
      setTyping(false);
      setQIndex(idx);
      addBot({ type:"question", question: QUESTIONS[idx] });
    }, 700);
  };

  const handleAnswer = (q, answer) => {
    const newAnswers = { ...answers, [q.key]: answer };
    setAnswers(newAnswers);
    addUser(Array.isArray(answer) ? answer.join(", ") : String(answer));

    const next = qIndex + 1;
    if (next < QUESTIONS.length) {
      askQ(next);
    } else {
      finalize(newAnswers);
    }
  };

  const finalize = (ans) => {
    setTyping(true);
    setTimeout(() => {
      setTyping(false);
      const sp = mapSpecialty(ans.mainSymptom, ans.ageGroup);
      const tv = tiageVal(ans.severity, ans.duration, ans.additional);
      const hospList = getHospitals(sp, ans.hospPref);
      setSpecialty(sp);
      setTriage(tv);
      setHospitals(hospList);
      setPhase("results");
      const tm = TRIAGE_META[tv];
      addBot({ type:"summary", specialty: sp, triage: tv, answers: ans });
      setTimeout(() => {
        addBot({ type:"hospital_list", hospitals: hospList, specialty: sp });
      }, 600);
    }, 1200);
  };

  const tiageVal = (sev, dur, add) => {
    const s = parseInt(sev) || 5;
    const hasBreath = Array.isArray(add) && add.includes("Difficulty breathing");
    const hasFever  = Array.isArray(add) && add.includes("High fever (>102°F)");
    if (s >= 9 || hasBreath) return "EMERGENCY";
    if (s >= 7 || (hasFever && dur !== "Just started today")) return "URGENT";
    if (s >= 4 || dur === "More than 2 weeks" || dur === "1–2 weeks") return "MODERATE";
    return "MILD";
  };

  const getHospitals = (sp, pref) => {
    let list = HOSPITALS[sp] || HOSPITALS.General;
    if (pref === "Government hospital (affordable)") list = list.filter(h => h.type === "Government");
    if (pref === "Private hospital (premium)") list = list.filter(h => h.type === "Private");
    if (list.length === 0) list = HOSPITALS[sp] || HOSPITALS.General;
    return list.sort((a,b) => b.rating - a.rating);
  };

  const handleHospSelect = (h) => {
    setSelectedHosp(h);
    const already = messages.some(m => m.type === "slots");
    if (!already) {
      setTimeout(() => {
        addBot({ type:"slots", hospital: h });
        setPhase("slots");
      }, 400);
    }
  };

  const confirmBooking = () => {
    const ref = "BLR" + Math.random().toString(36).substr(2,7).toUpperCase();
    setBooking({ hospital: selectedHosp, date: selectedDate, slot: selectedSlot, specialty, triage, ref });
    setScreen("success");
  };

  // ── RENDER QUESTION WIDGET ──────────────────────────────────────────────────
  const renderQ = (q) => {
    if (q.type === "quick") {
      return (
        <div style={{ display:"flex", flexWrap:"wrap", gap:"7px", marginTop:"10px" }}>
          {q.options.map(opt => (
            <button key={opt} onClick={() => handleAnswer(q, opt)} style={{
              padding:"8px 14px", borderRadius:"20px", border:"1.5px solid rgba(99,102,241,0.25)",
              background:"rgba(99,102,241,0.07)", color:"#4338CA", fontSize:"12px", fontWeight:"700",
              cursor:"pointer", transition:"all 0.15s"
            }}>{opt}</button>
          ))}
        </div>
      );
    }
    if (q.type === "scale") {
      return (
        <div style={{ marginTop:"10px" }}>
          <div style={{ display:"flex", gap:"6px", flexWrap:"wrap" }}>
            {[1,2,3,4,5,6,7,8,9,10].map(n => {
              const color = n <= 3 ? "#10B981" : n <= 6 ? "#F59E0B" : "#FF3B30";
              return (
                <button key={n} onClick={() => setScaleSel(n)} style={{
                  width:"38px", height:"38px", borderRadius:"10px", border:"1.5px solid",
                  borderColor: scaleSel === n ? color : "rgba(0,0,0,0.1)",
                  background: scaleSel === n ? color : "#fff",
                  color: scaleSel === n ? "#fff" : color,
                  fontWeight:"800", fontSize:"14px", cursor:"pointer", transition:"all 0.15s"
                }}>{n}</button>
              );
            })}
          </div>
          {scaleSel && (
            <button onClick={() => { handleAnswer(q, scaleSel); setScaleSel(null); }} style={{
              marginTop:"10px", padding:"10px 24px", borderRadius:"12px", border:"none",
              background:"linear-gradient(135deg,#6366F1,#4F46E5)", color:"#fff",
              fontWeight:"700", fontSize:"13px", cursor:"pointer"
            }}>Confirm →</button>
          )}
        </div>
      );
    }
    if (q.type === "multi") {
      const done = multiSel.length > 0;
      return (
        <div style={{ marginTop:"10px" }}>
          <div style={{ display:"flex", flexWrap:"wrap", gap:"7px" }}>
            {q.options.map(opt => {
              const sel = multiSel.includes(opt);
              return (
                <button key={opt} onClick={() => setMultiSel(prev => sel ? prev.filter(x=>x!==opt) : [...prev, opt])} style={{
                  padding:"8px 14px", borderRadius:"20px", border:"1.5px solid",
                  borderColor: sel ? "#6366F1" : "rgba(0,0,0,0.1)",
                  background: sel ? "rgba(99,102,241,0.1)" : "#fff",
                  color: sel ? "#4338CA" : "#6B7280",
                  fontSize:"12px", fontWeight:"700", cursor:"pointer", transition:"all 0.15s"
                }}>
                  {sel ? "✓ " : ""}{opt}
                </button>
              );
            })}
          </div>
          {done && (
            <button onClick={() => { handleAnswer(q, multiSel); setMultiSel([]); }} style={{
              marginTop:"10px", padding:"10px 24px", borderRadius:"12px", border:"none",
              background:"linear-gradient(135deg,#6366F1,#4F46E5)", color:"#fff",
              fontWeight:"700", fontSize:"13px", cursor:"pointer"
            }}>Next →</button>
          )}
        </div>
      );
    }
  };

  const progress = Math.round((qIndex / QUESTIONS.length) * 100);

  return (
    <div style={{ minHeight:"100vh", fontFamily:"'Nunito','Segoe UI',sans-serif", background:"#F8F7FF", color:"#111827", position:"relative" }}>
      <div style={{ position:"fixed", inset:0, background:"linear-gradient(135deg,#EEF2FF 0%,#F0FDF4 60%,#FFF7ED 100%)", zIndex:0 }} />
      <div style={{ position:"fixed", top:"-100px", right:"-100px", width:"400px", height:"400px", borderRadius:"50%", background:"radial-gradient(circle,rgba(99,102,241,0.1) 0%,transparent 70%)", zIndex:0 }} />
      <div style={{ position:"fixed", bottom:"-80px", left:"-80px", width:"300px", height:"300px", borderRadius:"50%", background:"radial-gradient(circle,rgba(16,185,129,0.08) 0%,transparent 70%)", zIndex:0 }} />

      {/* ── HOME ── */}
      {screen === "home" && (
        <div style={{ position:"relative", zIndex:1, maxWidth:"420px", margin:"0 auto", padding:"32px 20px", minHeight:"100vh", display:"flex", flexDirection:"column" }}>
          <div style={{ flex:1 }}>
            <div style={{ textAlign:"center", padding:"44px 0 28px" }}>
              <div style={{ width:"80px", height:"80px", borderRadius:"24px", margin:"0 auto 18px", background:"linear-gradient(135deg,#6366F1,#10B981)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:"40px", boxShadow:"0 16px 40px rgba(99,102,241,0.3)" }}>🩺</div>
              <h1 style={{ fontSize:"32px", fontWeight:"900", letterSpacing:"-1px", margin:"0 0 6px", color:"#111827" }}>
                Medi<span style={{ color:"#6366F1" }}>Connect</span>
              </h1>
              <p style={{ color:"#6B7280", fontSize:"14px", margin:"0 0 4px" }}>AI-Powered Hospital Booking</p>
              <div style={{ display:"inline-flex", alignItems:"center", gap:"5px", padding:"4px 12px", borderRadius:"20px", background:"rgba(99,102,241,0.08)", border:"1px solid rgba(99,102,241,0.2)" }}>
                <span style={{ fontSize:"12px" }}>📍</span>
                <span style={{ fontSize:"12px", fontWeight:"700", color:"#6366F1" }}>Bangalore, Karnataka</span>
              </div>
            </div>

            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr 1fr", gap:"10px", marginBottom:"24px" }}>
              {[["50+","Hospitals"],["4.7★","Avg Rating"],["24/7","Support"]].map(([v,l]) => (
                <div key={l} style={{ padding:"16px 10px", borderRadius:"16px", background:"#fff", boxShadow:"0 2px 12px rgba(0,0,0,0.06)", textAlign:"center" }}>
                  <div style={{ fontWeight:"900", fontSize:"18px", color:"#6366F1" }}>{v}</div>
                  <div style={{ fontSize:"11px", color:"#9CA3AF", marginTop:"2px" }}>{l}</div>
                </div>
              ))}
            </div>

            <button onClick={startChat} style={{
              width:"100%", padding:"18px", borderRadius:"16px", border:"none",
              background:"linear-gradient(135deg,#6366F1,#4F46E5)", color:"#fff",
              fontWeight:"900", fontSize:"16px", cursor:"pointer", letterSpacing:"0.3px",
              boxShadow:"0 8px 28px rgba(99,102,241,0.4)", display:"flex", alignItems:"center", justifyContent:"center", gap:"10px"
            }}>💬 Start Health Assessment</button>

            <div style={{ marginTop:"22px" }}>
              <div style={{ fontSize:"11px", fontWeight:"800", color:"#9CA3AF", letterSpacing:"1.5px", marginBottom:"12px" }}>BROWSE BY SPECIALTY</div>
              <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:"10px" }}>
                {[
                  ["❤️","Cardiology","Heart & vascular"],
                  ["🧠","Neurology","Brain & nerves"],
                  ["🦴","Orthopedics","Bones & joints"],
                  ["👶","Pediatrics","Child healthcare"],
                  ["🫁","Pulmonology","Lung & breathing"],
                  ["🧴","Dermatology","Skin conditions"],
                ].map(([icon,title,desc]) => (
                  <div key={title} onClick={startChat} style={{
                    padding:"14px", borderRadius:"14px", background:"#fff", cursor:"pointer",
                    boxShadow:"0 2px 10px rgba(0,0,0,0.05)", display:"flex", alignItems:"center", gap:"10px",
                    border:"1.5px solid transparent", transition:"all 0.2s"
                  }}>
                    <span style={{ fontSize:"22px" }}>{icon}</span>
                    <div>
                      <div style={{ fontWeight:"800", fontSize:"13px", color:"#111827" }}>{title}</div>
                      <div style={{ fontSize:"11px", color:"#9CA3AF" }}>{desc}</div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
          <div style={{ textAlign:"center", paddingTop:"24px", fontSize:"11px", color:"#D1D5DB" }}>MediConnect • Bangalore • Powered by AI</div>
        </div>
      )}

      {/* ── CHAT ── */}
      {screen === "chat" && (
        <div style={{ position:"relative", zIndex:1, maxWidth:"440px", margin:"0 auto", display:"flex", flexDirection:"column", height:"100vh" }}>
          {/* Topbar */}
          <div style={{ padding:"12px 16px", background:"rgba(255,255,255,0.95)", backdropFilter:"blur(12px)", borderBottom:"1px solid rgba(0,0,0,0.07)", display:"flex", alignItems:"center", gap:"12px", position:"sticky", top:0, zIndex:10 }}>
            <button onClick={() => setScreen("home")} style={{ background:"none", border:"none", cursor:"pointer", fontSize:"20px", color:"#6366F1", padding:0 }}>←</button>
            <div style={{ width:"36px", height:"36px", borderRadius:"12px", background:"linear-gradient(135deg,#6366F1,#10B981)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:"18px" }}>🤖</div>
            <div style={{ flex:1 }}>
              <div style={{ fontWeight:"900", fontSize:"15px" }}>MediBot</div>
              <div style={{ fontSize:"10px", color:"#10B981", fontWeight:"700" }}>● AI Health Assistant • Bangalore</div>
            </div>
            {phase === "intake" && (
              <div style={{ textAlign:"right" }}>
                <div style={{ fontSize:"10px", color:"#9CA3AF", fontWeight:"700" }}>Q {Math.min(qIndex+1,QUESTIONS.length)}/{QUESTIONS.length}</div>
                <div style={{ width:"60px", height:"4px", background:"#E5E7EB", borderRadius:"2px", marginTop:"3px", overflow:"hidden" }}>
                  <div style={{ width:`${progress}%`, height:"100%", background:"linear-gradient(90deg,#6366F1,#10B981)", borderRadius:"2px", transition:"width 0.3s" }} />
                </div>
              </div>
            )}
          </div>

          {/* Messages */}
          <div style={{ flex:1, overflowY:"auto", padding:"14px" }}>
            {messages.map((msg, i) => (
              <div key={i} style={{ marginBottom:"12px", display:"flex", flexDirection:"column", alignItems: msg.role==="user" ? "flex-end" : "flex-start" }}>
                {msg.role === "bot" && (
                  <div style={{ display:"flex", gap:"8px", alignItems:"flex-start", width:"100%" }}>
                    <div style={{ width:"28px", height:"28px", borderRadius:"10px", background:"linear-gradient(135deg,#6366F1,#10B981)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:"13px", flexShrink:0, marginTop:"2px" }}>🤖</div>
                    <div style={{ flex:1 }}>

                      {/* Text bubble */}
                      {msg.type === "text" && (
                        <div style={{ background:"#fff", borderRadius:"4px 16px 16px 16px", padding:"12px 16px", maxWidth:"88%", boxShadow:"0 2px 10px rgba(0,0,0,0.07)", fontSize:"14px", lineHeight:"1.6", color:"#1F2937" }}
                          dangerouslySetInnerHTML={{ __html: fmt(msg.text) }} />
                      )}

                      {/* Question bubble */}
                      {msg.type === "question" && (
                        <div style={{ width:"100%" }}>
                          <div style={{ background:"#fff", borderRadius:"4px 16px 16px 16px", padding:"14px 16px", boxShadow:"0 2px 10px rgba(0,0,0,0.07)", marginBottom:"8px" }}>
                            <div style={{ fontSize:"14px", lineHeight:"1.6", color:"#1F2937" }} dangerouslySetInnerHTML={{ __html: fmt(msg.question.text) }} />
                          </div>
                          {i === messages.length - 1 && renderQ(msg.question)}
                        </div>
                      )}

                      {/* Summary */}
                      {msg.type === "summary" && (() => {
                        const tm = TRIAGE_META[msg.triage];
                        return (
                          <div style={{ background:"#fff", borderRadius:"4px 16px 16px 16px", padding:"16px", boxShadow:"0 2px 10px rgba(0,0,0,0.07)", maxWidth:"95%" }}>
                            <div style={{ fontWeight:"800", fontSize:"13px", color:"#111827", marginBottom:"10px" }}>📋 Assessment Summary</div>
                            <div style={{ display:"flex", gap:"8px", flexWrap:"wrap", marginBottom:"10px" }}>
                              <TriagePill level={msg.triage} />
                              <span style={{ padding:"3px 10px", borderRadius:"20px", fontSize:"11px", fontWeight:"700", background:"rgba(99,102,241,0.1)", color:"#6366F1" }}>🩺 {msg.specialty}</span>
                            </div>
                            <div style={{ background: tm.bg, borderRadius:"10px", padding:"10px 12px", fontSize:"12px", color: tm.color, fontWeight:"700", lineHeight:"1.5" }}>
                              {tm.icon} {tm.label}
                            </div>
                            <div style={{ marginTop:"10px", display:"grid", gridTemplateColumns:"1fr 1fr", gap:"6px" }}>
                              {[
                                ["Symptom", msg.answers.mainSymptom],
                                ["Duration", msg.answers.duration],
                                ["Severity", `${msg.answers.severity}/10`],
                                ["Age Group", msg.answers.ageGroup],
                              ].map(([k,v]) => (
                                <div key={k} style={{ background:"#F9FAFB", borderRadius:"8px", padding:"7px 10px" }}>
                                  <div style={{ fontSize:"10px", color:"#9CA3AF", fontWeight:"700" }}>{k.toUpperCase()}</div>
                                  <div style={{ fontSize:"12px", fontWeight:"800", color:"#111827", marginTop:"2px" }}>{v}</div>
                                </div>
                              ))}
                            </div>
                          </div>
                        );
                      })()}

                      {/* Hospital list */}
                      {msg.type === "hospital_list" && (
                        <div style={{ width:"100%" }}>
                          <div style={{ background:"#fff", borderRadius:"4px 16px 16px 16px", padding:"12px 16px", boxShadow:"0 2px 10px rgba(0,0,0,0.07)", marginBottom:"10px", fontSize:"14px", color:"#1F2937", lineHeight:"1.6" }}>
                            Here are the <strong>top {msg.hospitals.length} hospitals</strong> in Bangalore for <strong>{msg.specialty}</strong>. Tap one to select it 👇
                          </div>
                          {msg.hospitals.map(h => (
                            <HospCard key={h.id} h={h} selected={selectedHosp?.id === h.id} onSelect={handleHospSelect} />
                          ))}
                        </div>
                      )}

                      {/* Slot picker */}
                      {msg.type === "slots" && (
                        <div style={{ width:"100%" }}>
                          <div style={{ background:"#fff", borderRadius:"4px 16px 16px 16px", padding:"12px 16px", boxShadow:"0 2px 10px rgba(0,0,0,0.07)", marginBottom:"10px", fontSize:"14px", color:"#1F2937", lineHeight:"1.6" }}>
                            Excellent choice! 🎉 Now pick a <strong>date and time</strong> for your appointment at <strong>{msg.hospital.name}</strong>.
                          </div>

                          {/* Date */}
                          <div style={{ background:"#fff", borderRadius:"14px", padding:"14px", boxShadow:"0 2px 10px rgba(0,0,0,0.06)", marginBottom:"10px" }}>
                            <div style={{ fontSize:"10px", fontWeight:"800", color:"#9CA3AF", letterSpacing:"1.5px", marginBottom:"10px" }}>SELECT DATE</div>
                            <div style={{ display:"flex", gap:"8px", overflowX:"auto", paddingBottom:"4px" }}>
                              {dates.map(d => (
                                <button key={d.val} onClick={() => setSelectedDate(d.label)} style={{
                                  flexShrink:0, padding:"8px 12px", borderRadius:"12px", border:"1.5px solid",
                                  borderColor: selectedDate === d.label ? "#6366F1" : "rgba(0,0,0,0.08)",
                                  background: selectedDate === d.label ? "#EEF2FF" : "#fff",
                                  color: selectedDate === d.label ? "#6366F1" : "#6B7280",
                                  fontWeight:"700", fontSize:"11px", cursor:"pointer", whiteSpace:"nowrap"
                                }}>{d.label}</button>
                              ))}
                            </div>
                          </div>

                          {/* Time */}
                          {selectedDate && (
                            <div style={{ background:"#fff", borderRadius:"14px", padding:"14px", boxShadow:"0 2px 10px rgba(0,0,0,0.06)", marginBottom:"10px" }}>
                              <div style={{ fontSize:"10px", fontWeight:"800", color:"#9CA3AF", letterSpacing:"1.5px", marginBottom:"10px" }}>AVAILABLE SLOTS</div>
                              <div style={{ display:"grid", gridTemplateColumns:"repeat(3,1fr)", gap:"7px" }}>
                                {TIMESLOTS.map(t => (
                                  <button key={t} onClick={() => setSelectedSlot(t)} style={{
                                    padding:"10px 4px", borderRadius:"10px", border:"1.5px solid",
                                    borderColor: selectedSlot === t ? "#10B981" : "rgba(0,0,0,0.08)",
                                    background: selectedSlot === t ? "#F0FDF4" : "#fff",
                                    color: selectedSlot === t ? "#10B981" : "#6B7280",
                                    fontWeight:"700", fontSize:"12px", cursor:"pointer"
                                  }}>{t}</button>
                                ))}
                              </div>
                            </div>
                          )}

                          {selectedDate && selectedSlot && (
                            <button onClick={confirmBooking} style={{
                              width:"100%", padding:"16px", borderRadius:"14px", border:"none",
                              background:"linear-gradient(135deg,#10B981,#059669)", color:"#fff",
                              fontWeight:"900", fontSize:"15px", cursor:"pointer",
                              boxShadow:"0 6px 20px rgba(16,185,129,0.35)"
                            }}>✅ Confirm Appointment</button>
                          )}
                        </div>
                      )}

                    </div>
                  </div>
                )}

                {msg.role === "user" && (
                  <div style={{ background:"linear-gradient(135deg,#6366F1,#4F46E5)", color:"#fff", borderRadius:"16px 16px 4px 16px", padding:"11px 16px", maxWidth:"75%", fontSize:"13px", lineHeight:"1.5", boxShadow:"0 4px 12px rgba(99,102,241,0.3)", fontWeight:"600" }}>
                    {msg.text}
                  </div>
                )}
              </div>
            ))}

            {typing && (
              <div style={{ display:"flex", gap:"8px", alignItems:"flex-start", marginBottom:"12px" }}>
                <div style={{ width:"28px", height:"28px", borderRadius:"10px", background:"linear-gradient(135deg,#6366F1,#10B981)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:"13px" }}>🤖</div>
                <div style={{ background:"#fff", borderRadius:"4px 16px 16px 16px", padding:"14px 18px", boxShadow:"0 2px 10px rgba(0,0,0,0.07)" }}>
                  <div style={{ display:"flex", gap:"5px", alignItems:"center" }}>
                    {[0,1,2].map(i => (
                      <div key={i} style={{ width:"8px", height:"8px", borderRadius:"50%", background:"#6366F1", animation:`bounce 1s ease ${i*0.2}s infinite` }} />
                    ))}
                  </div>
                </div>
              </div>
            )}
            <div ref={chatEndRef} />
          </div>

          {/* Sticky disclaimer */}
          <div style={{ padding:"10px 16px 14px", background:"rgba(255,255,255,0.9)", backdropFilter:"blur(10px)", borderTop:"1px solid rgba(0,0,0,0.06)" }}>
            <div style={{ fontSize:"10px", color:"#9CA3AF", textAlign:"center", lineHeight:"1.5" }}>
              ⚕️ MediBot provides guidance only — not a substitute for professional medical advice.
            </div>
          </div>
        </div>
      )}

      {/* ── SUCCESS ── */}
      {screen === "success" && booking && (() => {
        const tm = TRIAGE_META[booking.triage];
        return (
          <div style={{ position:"relative", zIndex:1, maxWidth:"420px", margin:"0 auto", padding:"32px 20px", minHeight:"100vh", display:"flex", flexDirection:"column", justifyContent:"center" }}>
            <div style={{ textAlign:"center", marginBottom:"28px" }}>
              <div style={{ fontSize:"70px", marginBottom:"12px" }}>🎉</div>
              <h2 style={{ fontSize:"26px", fontWeight:"900", letterSpacing:"-0.5px", margin:"0 0 6px" }}>Appointment Confirmed!</h2>
              <p style={{ color:"#6B7280", margin:0, fontSize:"14px" }}>Your booking is all set in Bangalore</p>
            </div>

            <div style={{ background:"#fff", borderRadius:"20px", padding:"22px", boxShadow:"0 4px 24px rgba(0,0,0,0.08)", marginBottom:"14px" }}>
              <div style={{ display:"flex", alignItems:"center", gap:"12px", paddingBottom:"14px", borderBottom:"1px solid #F3F4F6", marginBottom:"14px" }}>
                <div style={{ width:"46px", height:"46px", borderRadius:"14px", fontSize:"22px", background:"#EEF2FF", display:"flex", alignItems:"center", justifyContent:"center" }}>{booking.hospital.img}</div>
                <div>
                  <div style={{ fontWeight:"900", fontSize:"15px" }}>{booking.hospital.name}</div>
                  <div style={{ fontSize:"12px", color:"#9CA3AF" }}>📍 {booking.hospital.area}, Bangalore</div>
                  <div style={{ fontSize:"11px", color:"#6B7280" }}>{booking.hospital.address}</div>
                </div>
              </div>
              {[
                ["🩺 Specialty", booking.specialty],
                ["📅 Date", booking.date],
                ["⏰ Time", booking.slot],
                ["📞 Phone", booking.hospital.phone],
                ["⭐ Rating", `${booking.hospital.rating} (${booking.hospital.reviews.toLocaleString()} reviews)`],
              ].map(([k,v]) => (
                <div key={k} style={{ display:"flex", justifyContent:"space-between", padding:"9px 0", borderBottom:"1px solid #F9FAFB" }}>
                  <span style={{ fontSize:"13px", color:"#9CA3AF" }}>{k}</span>
                  <span style={{ fontSize:"13px", fontWeight:"700", color:"#111827" }}>{v}</span>
                </div>
              ))}
              <div style={{ marginTop:"14px", background:tm.bg, borderRadius:"12px", padding:"10px 14px", display:"flex", alignItems:"center", gap:"8px" }}>
                <span style={{ fontSize:"16px" }}>{tm.icon}</span>
                <div>
                  <div style={{ fontSize:"10px", fontWeight:"800", color: tm.color, letterSpacing:"1px" }}>TRIAGE LEVEL</div>
                  <div style={{ fontSize:"12px", fontWeight:"700", color: tm.color }}>{tm.label}</div>
                </div>
              </div>
              <div style={{ marginTop:"12px", background:"#EEF2FF", borderRadius:"12px", padding:"12px 16px", textAlign:"center" }}>
                <div style={{ fontSize:"10px", color:"#6366F1", fontWeight:"800", letterSpacing:"1.5px" }}>BOOKING REFERENCE</div>
                <div style={{ fontSize:"24px", fontWeight:"900", color:"#4338CA", letterSpacing:"3px", marginTop:"4px" }}>{booking.ref}</div>
              </div>
            </div>

            <button onClick={() => { setScreen("home"); }} style={{
              width:"100%", padding:"16px", borderRadius:"16px", border:"none",
              background:"linear-gradient(135deg,#6366F1,#4F46E5)", color:"#fff",
              fontWeight:"900", fontSize:"15px", cursor:"pointer", boxShadow:"0 8px 24px rgba(99,102,241,0.35)"
            }}>← Back to Home</button>
          </div>
        );
      })()}

      <style>{`
        @keyframes bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-6px)} }
        *{box-sizing:border-box;margin:0;padding:0}
        ::-webkit-scrollbar{width:4px;height:4px}
        ::-webkit-scrollbar-thumb{background:rgba(0,0,0,0.1);border-radius:4px}
      `}</style>
    </div>
  );
}
