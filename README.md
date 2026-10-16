
                        <!DOCTYPE html>
                        <html lang="en">
                        <head>
                            <meta charset="UTF-8">
                            <meta name="viewport" content="width=device-width, initial-scale=1.0">
              <style>
                body {
                  background-color: white; /* Ensure the iframe has a white background */
                }

                :root {
    --bg-color: #050505;
    --terminal-bg: #0a0a0a;
    --primary-color: #00ff41; /* Hacker green */
    --secondary-color: #00f3ff; /* Cyan */
    --text-color: #e0e0e0;
    --dim-color: #888;
    --border-color: #00ff41;
    --font-mono: 'Fira Code', 'Courier New', monospace;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
    background-color: var(--bg-color);
    color: var(--text-color);
    font-family: var(--font-mono);
    line-height: 1.6;
    overflow-x: hidden;
}

#matrix-canvas {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    z-index: -1; opacity: 0.15;
}

.scanlines {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: linear-gradient(to bottom, rgba(255,255,255,0), rgba(255,255,255,0) 50%, rgba(0,0,0,0.2) 50%, rgba(0,0,0,0.2));
    background-size: 100% 4px;
    z-index: 1000; pointer-events: none; opacity: 0.3;
}

.container { max-width: 900px; margin: 0 auto; padding: 2rem 1rem; position: relative; z-index: 1; }

.terminal-window {
    background: var(--terminal-bg);
    border: 1px solid var(--border-color);
    border-radius: 8px;
    margin-bottom: 2rem;
    box-shadow: 0 0 10px rgba(0, 255, 65, 0.2);
    overflow: hidden;
    transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.terminal-window:hover {
    box-shadow: 0 0 20px rgba(0, 255, 65, 0.4);
    transform: translateY(-2px);
}

.terminal-header {
    background: #1a1a1a; padding: 0.5rem 1rem; display: flex; align-items: center;
    border-bottom: 1px solid var(--border-color);
}

.dot { width: 12px; height: 12px; border-radius: 50%; display: inline-block; margin-right: 8px; }
.red { background: #ff5f56; } .yellow { background: #ffbd2e; } .green { background: #27c93f; }
.title { margin-left: 1rem; color: var(--dim-color); font-size: 0.9rem; }
.terminal-body { padding: 1.5rem; }

.typing-svg { max-width: 100%; margin-bottom: 1rem; }

h1.glitch {
    font-size: 2.5rem; color: var(--primary-color); position: relative;
    display: inline-block; margin-bottom: 0.5rem;
}

h1.glitch::before, h1.glitch::after {
    content: attr(data-text); position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: var(--terminal-bg);
}

h1.glitch::before { left: 2px; text-shadow: -1px 0 #ff00c1; clip: rect(44px, 450px, 56px, 0); animation: glitch-anim 5s infinite linear alternate-reverse; }
h1.glitch::after { left: -2px; text-shadow: -1px 0 #00fff9; clip: rect(44px, 450px, 56px, 0); animation: glitch-anim2 5s infinite linear alternate-reverse; }

@keyframes glitch-anim {
    0% { clip: rect(31px, 9999px, 94px, 0); } 20% { clip: rect(78px, 9999px, 20px, 0); }
    40% { clip: rect(12px, 9999px, 6px, 0); } 60% { clip: rect(65px, 9999px, 89px, 0); }
    80% { clip: rect(23px, 9999px, 67px, 0); } 100% { clip: rect(91px, 9999px, 34px, 0); }
}
@keyframes glitch-anim2 {
    0% { clip: rect(65px, 9999px, 99px, 0); } 20% { clip: rect(12px, 9999px, 45px, 0); }
    40% { clip: rect(89px, 9999px, 12px, 0); } 60% { clip: rect(34px, 9999px, 78px, 0); }
    80% { clip: rect(56px, 9999px, 23px, 0); } 100% { clip: rect(10px, 9999px, 88px, 0); }
}

.roles { color: var(--secondary-color); font-size: 1.1rem; margin-bottom: 0.5rem; }
.motto { color: var(--primary-color); font-weight: bold; }
.cursor { animation: blink 1s step-end infinite; }
@keyframes blink { 50% { opacity: 0; } }

.checklist { list-style: none; }
.checklist li { margin-bottom: 0.8rem; display: flex; align-items: flex-start; }
.checklist li i { color: var(--primary-color); margin-right: 10px; margin-top: 4px; }

.cert-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1rem; }
.cert-card {
    background: rgba(0, 255, 65, 0.05); border: 1px solid rgba(0, 255, 65, 0.2);
    padding: 1rem; border-radius: 4px; transition: all 0.3s ease;
}
.cert-card:hover { background: rgba(0, 255, 65, 0.1); border-color: var(--primary-color); box-shadow: 0 0 10px rgba(0, 255, 65, 0.3); }
.cert-card h3 { color: var(--secondary-color); font-size: 1rem; margin-bottom: 0.5rem; }
.cert-card .issuer { display: block; color: var(--dim-color); font-size: 0.85rem; margin-bottom: 0.5rem; }
.cert-card .skills { font-size: 0.85rem; color: var(--text-color); }

.skill-category { margin-bottom: 1.5rem; }
.skill-category h4 { color: var(--primary-color); margin-bottom: 0.5rem; }
.tags { display: flex; flex-wrap: wrap; gap: 0.5rem; }
.tags span {
    background: rgba(0, 243, 255, 0.1); border: 1px solid var(--secondary-color);
    color: var(--secondary-color); padding: 0.3rem 0.8rem; border-radius: 4px;
    font-size: 0.85rem; transition: all 0.3s ease;
}
.tags span:hover { background: var(--secondary-color); color: var(--bg-color); box-shadow: 0 0 10px var(--secondary-color); }

.project-list { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1rem; }
.project-item {
    background: rgba(255, 255, 255, 0.03); border-left: 3px solid var(--primary-color);
    padding: 1rem; transition: all 0.3s ease;
}
.project-item:hover { background: rgba(0, 255, 65, 0.05); transform: translateX(5px); }
.project-item h3 { color: var(--text-color); margin-bottom: 0.5rem; }
.project-item p { color: var(--dim-color); font-size: 0.9rem; margin-bottom: 0.5rem; }
.tech-stack { display: inline-block; background: #1a1a1a; color: var(--primary-color); padding: 0.2rem 0.5rem; border-radius: 3px; font-size: 0.8rem; }

.center-content { text-align: center; }
.github-stats { max-width: 100%; border-radius: 6px; }

.social-links { display: flex; flex-wrap: wrap; gap: 1rem; justify-content: center; }
.social-links a {
    color: var(--text-color); text-decoration: none; padding: 0.5rem 1rem;
    border: 1px solid var(--dim-color); border-radius: 4px; transition: all 0.3s ease;
    display: flex; align-items: center; gap: 0.5rem;
}
.social-links a:hover {
    border-color: var(--primary-color); color: var(--primary-color);
    box-shadow: 0 0 10px rgba(0, 255, 65, 0.3); text-shadow: 0 0 5px var(--primary-color);
}

.resource-list { list-style: none; display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 0.5rem; }
.resource-list li a {
    color: var(--secondary-color); text-decoration: none; display: block;
    padding: 0.5rem; border-radius: 4px; transition: all 0.3s ease;
}
.resource-list li a:hover { background: rgba(0, 243, 255, 0.1); padding-left: 1rem; }

.coffee-btn { display: inline-block; transition: transform 0.3s ease; }
.coffee-btn:hover { transform: scale(1.05); filter: drop-shadow(0 0 10px rgba(255, 221, 0, 0.5)); }
.copyright { margin-top: 1.5rem; color: var(--dim-color); font-size: 0.85rem; }

@media (max-width: 600px) {
    h1.glitch { font-size: 1.8rem; }
    .roles { font-size: 0.9rem; }
    .terminal-body { padding: 1rem; }
}


              </style>
                        </head>
                        <body>
                            <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ifeanyi Ekezie | System Profile</title>
    <link rel="stylesheet" href="style.css">
    <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;600&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <canvas id="matrix-canvas"></canvas>
    <div class="scanlines"></div>
    
    <div class="container">
        <!-- Header -->
        <header class="terminal-window">
            <div class="terminal-header">
                <span class="dot red"></span>
                <span class="dot yellow"></span>
                <span class="dot green"></span>
                <span class="title">ifeanyi@kali:~./profile</span>
            </div>
            <div class="terminal-body">
                <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1000&color=00FF41&random=false&width=435&lines=Hey!+You+are+welcome+to+my+Profile;My+name+is+Ifeanyi+Ekezie;I+am+passionate+about+Technology;I+learn+by+Doing" alt="Typing SVG" class="typing-svg">
                <h1 class="glitch" data-text="Ifeanyi Ekezie">Ifeanyi Ekezie</h1>
                <p class="roles">> Software Engineer | Cybersecurity | Systems Administrator | Technical Support | IT Operations | Cloud Support</p>
                <p class="motto">> I Learn by Doing_<span class="cursor">█</span></p>
            </div>
        </header>

        <!-- Achievements -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./achievements.sh</span></div>
            <div class="terminal-body">
                <ul class="checklist">
                    <li><i class="fas fa-check-circle"></i> Completed <strong>Software Engineering</strong> at <strong>ALX</strong></li>
                    <li><i class="fas fa-check-circle"></i> Completed <strong>Microsoft Azure Fundamentals</strong> at <strong>3MTT Academy | Darey.io</strong></li>
                    <li><i class="fas fa-lock"></i> Completed <strong>Cybersecurity</strong> at <strong>ALX</strong></li>
                    <li><i class="fas fa-laptop-code"></i> Passionate about <strong>Backend Engineering</strong>, <strong>Cloud Security</strong>, and <strong>DevSecOps</strong></li>
                    <li><i class="fas fa-users"></i> Open to collaborate on <strong>real-world tech projects</strong> and <strong>community initiatives</strong></li>
                    <li><i class="fas fa-rocket"></i> Constantly learning by building and breaking things securely</li>
                </ul>
            </div>
        </section>

        <!-- Certifications -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./certifications.json</span></div>
            <div class="terminal-body">
                <div class="cert-grid">
                    <div class="cert-card">
                        <h3>🎓 Career Essentials in Generative AI</h3>
                        <span class="issuer">Microsoft & LinkedIn, Jul 2025</span>
                        <p class="skills">Prompt Engineering, Generative AI, Responsible AI, Copilot</p>
                    </div>
                    <div class="cert-card">
                        <h3>☁️ Cloud Computing Certification</h3>
                        <span class="issuer">3MTT Nigeria, Dec 2025</span>
                        <p class="skills">Microsoft Azure, Cloud Infrastructure, VMs, Cloud Security</p>
                    </div>
                    <div class="cert-card">
                        <h3>🧰 Google IT Support Professional</h3>
                        <span class="issuer">Coursera, Sep 2023 | ID: IHoAUjiJ</span>
                        <p class="skills">Network Admin, Troubleshooting, Linux/Windows OS</p>
                    </div>
                    <div class="cert-card">
                        <h3>🌐 Cisco Certified Network Professional (CCNP)</h3>
                        <span class="issuer">Feb 2026</span>
                        <p class="skills">Advanced Networking, Routing & Switching, Network Security</p>
                    </div>
                    <div class="cert-card">
                        <h3>🔐 Cybersecurity Program</h3>
                        <span class="issuer">ALX Africa, Jan 2026</span>
                        <p class="skills">Threat Analysis, Vulnerability Assessment, Incident Response</p>
                    </div>
                    <div class="cert-card">
                        <h3>💻 Software Development Program</h3>
                        <span class="issuer">Power Learn Project, Jul 2025</span>
                        <p class="skills">Python, JavaScript, Web Development, APIs, Git</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Skills -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./skills_and_tools.env</span></div>
            <div class="terminal-body">
                <div class="skill-category">
                    <h4>> Languages:</h4>
                    <div class="tags"><span>Go (Golang)</span><span>Python</span><span>JavaScript</span><span>Bash</span><span>SQL</span></div>
                </div>
                <div class="skill-category">
                    <h4>> Backend:</h4>
                    <div class="tags"><span>REST APIs</span><span>Microservices</span><span>Docker</span><span>Postgres</span><span>Redis</span></div>
                </div>
                <div class="skill-category">
                    <h4>> Cloud & DevOps:</h4>
                    <div class="tags"><span>Azure (AZ-900)</span><span>GitHub Actions</span><span>ARM Templates</span></div>
                </div>
                <div class="skill-category">
                    <h4>> Cybersecurity:</h4>
                    <div class="tags"><span>Network Security</span><span>OS Hardening</span><span>Vulnerability Scanning</span></div>
                </div>
            </div>
        </section>

        <!-- Projects -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./featured_projects.exe</span></div>
            <div class="terminal-body">
                <div class="project-list">
                    <div class="project-item">
                        <h3>🧠 LAGCleanBot</h3>
                        <p>AI-powered waste reporting platform</p>
                        <span class="tech-stack">Azure + Python + NLP</span>
                    </div>
                    <div class="project-item">
                        <h3>🌐 Cleaning Business MVP</h3>
                        <p>Full-stack booking platform w/ payments</p>
                        <span class="tech-stack">Next.js + Firebase</span>
                    </div>
                    <div class="project-item">
                        <h3>📊 Sentiment Analysis Dashboard</h3>
                        <p>Azure Cognitive Services + Python visualization</p>
                        <span class="tech-stack">Azure + Python</span>
                    </div>
                    <div class="project-item">
                        <h3>🔐 Azure Resource Organizer</h3>
                        <p>Infrastructure-as-Code mini-project using ARM templates</p>
                        <span class="tech-stack">Azure + ARM</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- GitHub Stats -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./github_stats.png</span></div>
            <div class="terminal-body center-content">
                <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=iiekezie&layout=compact&theme=github_dark" alt="Top Langs" class="github-stats">
            </div>
        </section>

        <!-- Social Links -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./connect.sh</span></div>
            <div class="terminal-body">
                <div class="social-links">
                    <a href="https://linkedin.com/in/iiekezie" target="_blank"><i class="fab fa-linkedin"></i> LinkedIn</a>
                    <a href="https://github.com/iiekezie" target="_blank"><i class="fab fa-github"></i> GitHub</a>
                    <a href="https://medium.com/@iiekezie" target="_blank"><i class="fab fa-medium"></i> Medium</a>
                    <a href="https://instagram.com/iiekezie" target="_blank"><i class="fab fa-instagram"></i> Instagram</a>
                    <a href="https://x.com/iiekezie" target="_blank"><i class="fab fa-x-twitter"></i> X (Twitter)</a>
                </div>
            </div>
        </section>

        <!-- Resources -->
        <section class="terminal-window">
            <div class="terminal-header"><span class="title">./resources.md</span></div>
            <div class="terminal-body">
                <ul class="resource-list">
                    <li><a href="https://stackoverflow.com/" target="_blank"><i class="fas fa-terminal"></i> Stack Overflow</a></li>
                    <li><a href="https://github.com/" target="_blank"><i class="fas fa-code-branch"></i> GitHub Discussions</a></li>
                    <li><a href="https://stackexchange.com/" target="_blank"><i class="fas fa-network-wired"></i> Stack Exchange</a></li>
                    <li><a href="https://codeproject.com/" target="_blank"><i class="fas fa-laptop-code"></i> CodeProject</a></li>
                    <li><a href="https://reddit.com/r/programming/" target="_blank"><i class="fab fa-reddit-alien"></i> Reddit: r/programming</a></li>
                </ul>
            </div>
        </section>

        <!-- Footer -->
        <footer class="terminal-window">
            <div class="terminal-header"><span class="title">./support_coffee.sh</span></div>
            <div class="terminal-body center-content">
                <a href="https://www.buymeacoffee.com/iiekezie" target="_blank" class="coffee-btn">
                    <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" height="50" width="210" alt="buymeacoffee" />
                </a>
                <p class="copyright">© 2026 Ifeanyi Ekezie. All systems operational.</p>
            </div>
        </footer>
    </div>

    <script src="script.js"></script>
</body>
</html>


              <script>
                              const express = require('express');
const path = require('path');
const app = express();
const PORT = process.env.PORT || 3000;

// Serve static files from the 'public' directory
app.use(express.static(path.join(__dirname, 'public')));

app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

app.listen(PORT, () => {
    console.log(`\x1b[32m[SYSTEM]\x1b[0m Server running at http://localhost:${PORT}`);
    console.log(`\x1b[36m[INFO]\x1b[0m Access the hacker profile in your browser.`);
});

// Matrix Rain Effect
const canvas = document.getElementById('matrix-canvas');
const ctx = canvas.getContext('2d');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const katakana = 'アァカサタナハマヤャラワガザダバパイィキシチニヒミリヂビピウゥクスツヌフムユュルグズブヅプエェケセテネヘメレゲゼデベペオォコソトノホモヨョロヲゴゾドボポヴッン';
const latin = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
const nums = '0123456789';
const alphabet = katakana + latin + nums;

const fontSize = 16;
const columns = canvas.width / fontSize;
const rainDrops = [];

for (let x = 0; x < columns; x++) {
    rainDrops[x] = 1;
}

const draw = () => {
    ctx.fillStyle = 'rgba(5, 5, 5, 0.05)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.fillStyle = '#00FF41'; // Hacker green text
    ctx.font = fontSize + 'px monospace';

    for (let i = 0; i < rainDrops.length; i++) {
        const text = alphabet.charAt(Math.floor(Math.random() * alphabet.length));
        ctx.fillText(text, i * fontSize, rainDrops[i] * fontSize);

        if (rainDrops[i] * fontSize > canvas.height && Math.random() > 0.975) {
            rainDrops[i] = 0;
        }
        rainDrops[i]++;
    }
};

setInterval(draw, 30);

window.addEventListener('resize', () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
});

// Interactive glitch effect on hover for cards
const glitchElements = document.querySelectorAll('.cert-card, .project-item');
glitchElements.forEach(el => {
    el.addEventListener('mouseenter', () => {
        el.style.textShadow = '2px 0 #ff00c1, -2px 0 #00fff9';
        setTimeout(() => {
            el.style.textShadow = 'none';
        }, 200);
    });
});

// Console greeting for fellow developers inspecting the page
console.log('%c[SYSTEM] Access granted.', 'color: #00ff41; font-size: 16px; font-weight: bold;');
console.log('%cWelcome to Ifeanyi Ekezie\'s Hacker Profile.', 'color: #00f3ff; font-size: 14px;');
console.log('%c> Initializing secure connection...', 'color: #888; font-size: 12px;');


              </script>
                        </body>
                        </html>
                    
