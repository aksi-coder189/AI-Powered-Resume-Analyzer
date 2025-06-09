# AI-Powered-Resume-Analyzer

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>AI-Powered Resume Analyzer</title>
  <style>
    /* CSS Variables */
    :root {
      --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica,
        Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
      --color-primary: #111827;
      --color-secondary: #6b7280;
      --color-accent: #000000;
      --color-accent-hover: #333333;
      --color-bg: #ffffff;
      --color-card-bg: #f9fafb;
      --shadow-card: 0 1px 3px rgb(0 0 0 / 0.1), 0 1px 2px rgb(0 0 0 / 0.06);
      --radius-md: 0.75rem;
      --transition-fast: 0.2s ease-in-out;
      --spacing-lg: 4rem;
      --spacing-xl: 5rem;
    }

    /* Reset and base */
    *, *::before, *::after {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: var(--font-sans);
      background-color: var(--color-bg);
      color: var(--color-secondary);
      line-height: 1.6;
      min-height: 100vh;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }
    h1, h2, h3, h4 {
      color: var(--color-primary);
      margin-top: 0;
      margin-bottom: 0.5em;
      font-weight: 700;
      line-height: 1.2;
      letter-spacing: -0.02em;
    }
    h1 {
      font-size: clamp(2.5rem, 5vw, 3.5rem);
      font-weight: 800;
    }
    h2 {
      font-size: 1.75rem;
      font-weight: 700;
    }
    p {
      font-size: 1rem;
      margin-top: 0;
      margin-bottom: 1rem;
      color: var(--color-secondary);
    }
    a {
      color: var(--color-accent);
      text-decoration: none;
      font-weight: 600;
      transition: color var(--transition-fast);
    }
    a:hover,
    a:focus {
      color: var(--color-accent-hover);
      outline: none;
    }

    /* Layout */
    .container {
      max-width: 1200px;
      padding-left: 1.25rem;
      padding-right: 1.25rem;
      margin-left: auto;
      margin-right: auto;
    }
    main {
      padding-top: var(--spacing-xl);
      padding-bottom: var(--spacing-xl);
    }
    /* Navigation */
    nav {
      position: sticky;
      top: 0;
      background: var(--color-bg);
      box-shadow: 0 1px 4px rgb(0 0 0 / 0.05);
      z-index: 100;
    }
    .nav-container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 1rem 1.5rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .logo {
      font-weight: 700;
      font-size: 1.25rem;
      color: var(--color-primary);
      letter-spacing: -0.015em;
      user-select: none;
      cursor: default;
    }
    .nav-links {
      display: flex;
      gap: 1.5rem;
      align-items: center;
    }
    .nav-links a {
      font-weight: 600;
      font-size: 0.95rem;
      padding: 0.25rem 0.5rem;
      border-radius: var(--radius-md);
      color: var(--color-secondary);
      transition: background-color var(--transition-fast), color var(--transition-fast);
    }
    .nav-links a:hover,
    .nav-links a:focus {
      background-color: #e5e7eb;
      color: var(--color-primary);
      outline: none;
    }

    /* Button Primary */
    .btn-primary {
      background-color: var(--color-accent);
      color: var(--color-bg);
      font-weight: 700;
      font-size: 1rem;
      padding: 0.75rem 1.5rem;
      border: none;
      border-radius: var(--radius-md);
      cursor: pointer;
      user-select: none;
      transition: background-color var(--transition-fast), transform var(--transition-fast);
      box-shadow: 0 4px 6px rgb(0 0 0 / 0.1);
    }
    .btn-primary:hover,
    .btn-primary:focus {
      background-color: var(--color-accent-hover);
      transform: scale(1.05);
      outline: none;
    }
    .btn-primary:active {
      transform: scale(0.98);
    }

    /* Hero Section */
    .hero {
      text-align: center;
      max-width: 700px;
      margin-left: auto;
      margin-right: auto;
      padding-bottom: var(--spacing-xl);
    }
    .hero p.subtext {
      font-size: 1.125rem;
      color: var(--color-secondary);
      margin-top: 0;
      margin-bottom: var(--spacing-lg);
      font-weight: 500;
    }

    /* Form */
    form {
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
      background-color: var(--color-card-bg);
      border-radius: var(--radius-md);
      box-shadow: var(--shadow-card);
      padding: 2rem 2.5rem;
    }
    label {
      display: block;
      font-weight: 600;
      margin-bottom: 0.5rem;
      color: var(--color-primary);
      font-size: 1rem;
    }
    input[type="file"] {
      border: 1px solid #d1d5db;
      border-radius: var(--radius-md);
      padding: 0.5rem;
      width: 100%;
      cursor: pointer;
      font-size: 1rem;
      color: var(--color-primary);
      background-color: var(--color-bg);
      transition: border-color var(--transition-fast);
      outline-offset: 2px;
    }
    input[type="file"]:focus {
      border-color: var(--color-accent);
      outline: none;
    }
    input[type="submit"] {
      margin-top: 1.5rem;
      width: 100%;
    }

    /* Results Section */
    #results {
      margin-top: var(--spacing-xl);
      max-width: 700px;
      margin-left: auto;
      margin-right: auto;
      display: grid;
      gap: 1.5rem;
      grid-template-columns: 1fr;
    }
    @media(min-width: 600px) {
      #results {
        grid-template-columns: repeat(2, 1fr);
      }
    }
    .card {
      background-color: var(--color-card-bg);
      border-radius: var(--radius-md);
      padding: 1.5rem;
      box-shadow: var(--shadow-card);
      display: flex;
      flex-direction: column;
      gap: 0.75rem;
      transition: box-shadow var(--transition-fast);
    }
    .card:hover,
    .card:focus-within {
      box-shadow: 0 10px 15px rgb(0 0 0 / 0.15);
      outline: none;
    }
    .card h3 {
      color: var(--color-primary);
      font-weight: 700;
      font-size: 1.25rem;
      margin-bottom: 0.25rem;
    }
    .card ul {
      margin: 0;
      padding-left: 1.25rem;
      color: var(--color-secondary);
      font-size: 0.95rem;
    }
    .card ul li {
      margin-bottom: 0.25rem;
    }

    /* Accessibility Focus */
    :focus-visible {
      outline: 2px solid var(--color-accent);
      outline-offset: 3px;
    }
  </style>
</head>
<body>
  <nav role="navigation" aria-label="Primary navigation">
    <div class="nav-container container">
      <div class="logo" tabindex="0" aria-label="AI Powered Resume Analyzer Logo">ResumeAI</div>
      <div class="nav-links">
        <a href="#upload" role="link">Upload Resume</a>
        <a href="#results" role="link">Analysis Results</a>
      </div>
    </div>
  </nav>

  <main class="container" role="main">
    <section class="hero" aria-labelledby="hero-title" tabindex="-1">
      <h1 id="hero-title">AI-Powered Resume Analyzer</h1>
      <p class="subtext">Quickly analyze your resume and get insights on skills, experience, and keywords optimized for recruiters.</p>
      <button class="btn-primary" type="button" onclick="scrollToUpload()">Get Started</button>
    </section>

    <section id="upload" aria-labelledby="upload-title" tabindex="-1">
      <h2 id="upload-title">Upload Your Resume</h2>
      <form id="resume-form" aria-describedby="upload-description" novalidate>
        <p id="upload-description" style="color:#6b7280;">Supported formats: Plain text (.txt) or PDF (.pdf)</p>
        <label for="resume-file">Choose a file:</label>
        <input type="file" id="resume-file" name="resume-file" accept=".txt,.pdf" required aria-required="true" aria-describedby="file-error" />
        <span id="file-error" style="color: #dc2626; display: none;"></span>
        <input type="submit" value="Analyze Resume" class="btn-primary" aria-label="Analyze Resume" />
      </form>
    </section>

    <section id="results" aria-live="polite" aria-atomic="true" tabindex="-1" aria-label="Analysis results">
      <!-- Analysis results will be rendered here -->
    </section>
  </main>

  <script>
    // Utility to scroll to upload section
    function scrollToUpload() {
      const uploadSection = document.getElementById('upload');
      uploadSection.scrollIntoView({ behavior: 'smooth' });
      // Focus on file input for accessibility
      document.getElementById('resume-file').focus();
    }

    // Simple PDF parsing helper (extract text) depends on PDF.js or similar external lib is not included here.
    // So we will only accept plain text files for the demo. PDFs will show unsupported for now.

    // Minimal skill keywords list for demo
    const skillKeywords = [
      "JavaScript", "Python", "Java", "SQL", "C++",
      "React", "Node.js", "AWS", "Docker", "Kubernetes",
      "Machine Learning", "Data Science", "Git", "Linux",
      "Communication", "Leadership", "Teamwork", "Problem Solving"
    ];

    // Analyzes text content extracting skills occurrences and basic info
    function analyzeResumeText(text) {
      const lowerText = text.toLowerCase();

      // Count identified keywords
      let skillsFound = skillKeywords.filter(skill =>
        lowerText.includes(skill.toLowerCase())
      );

      // Find frequency count for each skill (optional)
      const freqs = {};
      skillsFound.forEach(skill => {
        const regex = new RegExp('\\b' + skill.replace(/\./g,'\\.') + '\\b', 'gi');
        const matches = text.match(regex);
        freqs[skill] = matches ? matches.length : 0;
      });

      // Basic mock summary info
      const lines = text.split('\n').filter(l => l.trim().length > 0);
      const numLines = lines.length;
      const numWords = text.trim().split(/\s+/).length;

      return {
        skills: freqs,
        wordCount: numWords,
        lineCount: numLines,
        rawTextSnippet: text.slice(0, 500) + (text.length > 500 ? "…" : "")
      };
    }

    // Utility to create a card element from title and content HTML
    function createCard(title, contentHTML) {
      const card = document.createElement('article');
      card.className = 'card';
      card.setAttribute('tabindex', '0');
      card.innerHTML = `
        <h3>${title}</h3>
        <div>${contentHTML}</div>
      `;
      return card;
    }

    // Render analysis results on the page
    function renderAnalysis(results) {
      const resultsSection = document.getElementById('results');
      resultsSection.innerHTML = '';

      // Summary Card
      const summaryHTML = `
        <p><strong>Word Count:</strong> ${results.wordCount}</p>
        <p><strong>Line Count:</strong> ${results.lineCount}</p>
        <p><strong>Resume Preview:</strong></p>
        <pre style="white-space: pre-wrap; font-family: monospace; color: #374151; border-radius: 0.5rem; background: #f3f4f6; padding: 1rem; max-height: 150px; overflow-y: auto;">${results.rawTextSnippet}</pre>
      `;
      resultsSection.appendChild(createCard('Summary', summaryHTML));

      // Skills Card
      if (Object.keys(results.skills).length > 0) {
        const skillsList = Object.entries(results.skills)
          .filter(([_, count]) => count > 0)
          .map(([skill, count]) => `<li>${skill} (${count})</li>`).join('');
        const skillsHTML = `<ul>${skillsList}</ul>`;
        resultsSection.appendChild(createCard('Detected Skills', skillsHTML));
      } else {
        resultsSection.appendChild(createCard('Detected Skills', '<p>No recognized skills found.</p>'));
      }
      // Focus newly added result section
      resultsSection.focus();
    }

    // Form validation and submit handler
    document.getElementById('resume-form').addEventListener('submit', function(event) {
      event.preventDefault();

      const fileInput = document.getElementById('resume-file');
      const errorSpan = document.getElementById('file-error');
      errorSpan.style.display = 'none';
      errorSpan.textContent = '';

      const file = fileInput.files[0];
      if (!file) {
        errorSpan.textContent = 'Please select a file.';
        errorSpan.style.display = 'block';
        fileInput.focus();
        return;
      }
      const fileName = file.name.toLowerCase();
      if (!(fileName.endsWith('.txt') || fileName.endsWith('.pdf'))) {
        errorSpan.textContent = 'Unsupported file format. Please upload a .txt or .pdf file.';
        errorSpan.style.display = 'block';
        fileInput.focus();
        return;
      }

      // For demo: Only handle text files. PDF will show not supported message.
      if (fileName.endsWith('.pdf')) {
        alert('PDF parsing is not supported in this demo. Please upload a plain text (.txt) resume.');
        return;
      }

      const reader = new FileReader();
      reader.onload = function(e) {
        const text = e.target.result;
        const analysis = analyzeResumeText(text);
        renderAnalysis(analysis);
      };
      reader.readAsText(file);
    });
  </script>
</body>
</html>

