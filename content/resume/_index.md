---
title: "🌁Me As A Picture"
weight: 4
toc: false
sidebar:
  hide: true
---

<style>
  .exp-row {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    flex-wrap: wrap;
    gap: 0.25rem;
  }
  @media (max-width: 600px) {
    .exp-row {
      flex-direction: column;
      align-items: flex-start;
    }
  }
</style>


## Contact

<div style="display: flex; flex-wrap: wrap; gap: 0.75rem; margin-top: 1rem; margin-bottom: 1rem;">
   <a href="Jack_Yao_Resume.pdf" target="_blank" rel="noopener noreferrer"
     style="display: inline-flex; align-items: center; gap: 0.4rem; padding: 0.5rem 1.1rem; background: #2563eb; color: #fff; border-radius: 6px; text-decoration: none; font-weight: 600; font-size: 0.95rem;">
     📋Résumé PDF
  </a>
  <a href="https://github.com/StarsExpress" target="_blank" rel="noopener noreferrer"
     style="display: inline-flex; align-items: center; gap: 0.4rem; padding: 0.5rem 1.1rem; border: 1.5px solid currentColor; border-radius: 6px; text-decoration: none; font-weight: 500; font-size: 0.95rem;">
    {{< icon "github" >}} GitHub
  </a>
  <a href="https://www.linkedin.com/in/Yuan-Jack-Yao" target="_blank" rel="noopener noreferrer"
     style="display: inline-flex; align-items: center; gap: 0.4rem; padding: 0.5rem 1.1rem; border: 1.5px solid currentColor; border-radius: 6px; text-decoration: none; font-weight: 500; font-size: 0.95rem;">
    {{< icon "linkedin" >}} LinkedIn
  </a>
  <a href="mailto:yyao411@gatech.edu"
     style="display: inline-flex; align-items: center; gap: 0.4rem; padding: 0.5rem 1.1rem; border: 1.5px solid currentColor; border-radius: 6px; text-decoration: none; font-weight: 500; font-size: 0.95rem;">
    ✉️ Email
  </a>
  <a href="tel:+14045286519"
     style="display: inline-flex; align-items: center; gap: 0.4rem; padding: 0.5rem 1.1rem; border: 1.5px solid currentColor; border-radius: 6px; text-decoration: none; font-weight: 500; font-size: 0.95rem;">
    📱 Phone
  </a>
</div>


## Work Experiences

[//]: # (GA Tech.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="gt_logo.png" alt="Georgia Institute of Technology" style="height: 30px; width: 30px; vertical-align: middle;">
    Georgia Institute of Technology
  </span>
  <span style="font-size: 16px; font-weight: 700;">Aug 2026～Dec 2026</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">CS 6604 Conversational AI Graduate Teaching Assistant</span>
  <span style="font-size: 15px; font-style: italic;">Atlanta, Georgia</span>
</div>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li><strong>Designed &amp; deployed assignments</strong> for an 80-student course:
  <strong>LoRA SFT + DPO</strong> with unit-test autograder; <strong>RAG with cross-session memory</strong>;
  held weekly office hours to guide students for debugging.</li>
</ul>

<br>

[//]: # (Walmart.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="walmart_logo.png" alt="Walmart" style="height: 30px; width: 30px; vertical-align: middle;">
    Walmart
  </span>
  <span style="font-size: 16px; font-weight: 700;">Jun 2026～Aug 2026</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">Data Scientist III Intern @ Sam's Club AI Acceleration Team</span>
  <span style="font-size: 15px; font-style: italic;">Bentonville, Arkansas</span>
</div>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Integrated <strong>LLM &amp; RAG with filtered vector search</strong> into dashboard for
  camera/area/site-level metadata queries, with <strong>cosine similarity &amp; LLMs-as-a-Judge</strong>
  against hallucinations, and an <strong>embedding signature</strong> verifying Milvus ingestion &amp;
  retrieval use the identical embedding model, <strong>eliminating silent retrieval degradation</strong>.</li>
</ul>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Architected a <strong>Cassandra-sourced</strong> data layer feeding <strong>PostgreSQL</strong>
  and <strong>Milvus</strong>, regenerable after data loss.</li>
</ul>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Deployed Docker &amp; Kubernetes CronJobs for <strong>1,245 in-store cameras across 9 sites</strong>:
  monthly Gemini Vision area classification with <strong>full-fleet inference in 13 minutes</strong>
  through high-throughput concurrency, glibc malloc arena caps and image pre-downsampling to
  <strong>improve reliability by eliminating OOM</strong>; hourly asynchronous snapshot capture to Azure,
  cutting <strong>50% time</strong> and <strong>75% storage cost</strong> via 7-day TTL.</li>
</ul>

<br>

[//]: # (SUEZ.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="suez_logo.png" alt="SUEZ" style="height: 30px; width: 80px; vertical-align: middle;">
    SUEZ
  </span>
  <span style="font-size: 16px; font-weight: 700;">May 2022～Sep 2024</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">Junior Data Scientist @ Smart and Environmental Solutions (SES) China</span>
  <span style="font-size: 15px; font-style: italic;">Shanghai, China</span>
</div>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Streamlined <strong>NLP classifier</strong> for <strong>client's imbalanced</strong>
  SCADA alarms, reaching <strong>82% production accuracy</strong>.</li>
</ul>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Designed a <strong>DBSCAN anomaly detector</strong> for <strong>client's</strong> air compressors predictive maintenance,
  achieving <strong>70% production F1 score</strong> and forecasting failures
  <strong>two weeks in advance despite having only a single feature</strong>.</li>
</ul>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Accelerated clustering on 12 air compressors with Joblib, <strong>reducing runtime by 20% in Linux CronJob.</strong></li>
</ul>

<br>

[//]: # (Pramerica Life Insurance.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="pramerica_logo.png" alt="Pramerica Life Insurance" style="height: 30px; width: 30px; vertical-align: middle;">
    Pramerica Life Insurance
  </span>
  <span style="font-size: 16px; font-weight: 700;">Sep 2020～Apr 2022</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">Sep～Dec 2020: Intern; Dec 2020～Apr 2022: Data Management Trainee</span>
  <span style="font-size: 15px; font-style: italic;">Shanghai, China</span>
</div>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Built the company's first BI sales data platform into both PC and WeChat mini programs.</li>
</ul>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
  <li>Developed multidimensional filtering, groupby & deduplication functions that saved 50% operation time.</li>
</ul>


## Skills Bag

<div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 1.25rem; margin-top: 1rem; align-items: stretch;">

<div style="padding: 1rem 1.25rem; border: 1px solid rgba(128,128,128,0.25); border-radius: 8px;">
  <div style="font-weight: 600; margin-bottom: 0.5rem;">⛓️ LLMs & Agents</div>
  AI Infra & Platform<br>RAG, Hugging Face<br>
  Fine-tuning, Distillation
</div>

<div style="padding: 1rem 1.25rem; border: 1px solid rgba(128,128,128,0.25); border-radius: 8px;">
  <div style="font-weight: 600; margin-bottom: 0.5rem;">🪄 Gen AI &amp; ML</div>
  <div style="display: flex; align-items: center; gap: 8px;">
    <img src="https://skillicons.dev/icons?i=pytorch,python,tensorflow" height="32" alt="Gen AI & ML" />
  </div>
</div>

<div style="padding: 1rem 1.25rem; border: 1px solid rgba(128,128,128,0.25); border-radius: 8px;">
  <div style="font-weight: 600; margin-bottom: 0.5rem;">🏎️ Automation & CI/CD</div>
  <div style="display: flex; align-items: center; gap: 8px;">
    <img src="https://skillicons.dev/icons?i=github,bitbucket,linux,docker,kubernetes" height="32" alt="Automation & CI/CD" />
  </div>
</div>

<div style="padding: 1rem 1.25rem; border: 1px solid rgba(128,128,128,0.25); border-radius: 8px;">
  <div style="font-weight: 600; margin-bottom: 0.5rem;">☁️ Cloud &amp; Data Platforms</div>
  <div style="display: flex; align-items: center; gap: 8px;">
    <img src="https://skillicons.dev/icons?i=azure,aws,gcp" height="32" alt="Cloud & Data Platforms" />
  </div>
</div>

<div style="padding: 1rem 1.25rem; border: 1px solid rgba(128,128,128,0.25); border-radius: 8px;">
  <div style="font-weight: 600; margin-bottom: 0.5rem;">🛢️ Databases & C++</div>
  <div style="display: flex; align-items: center; gap: 8px;">
    <img src="https://skillicons.dev/icons?i=cassandra,mysql,postgresql,cpp" height="32" alt="Databases & C++" />
  </div>
</div>

<div style="padding: 1rem 1.25rem; border: 1px solid rgba(128,128,128,0.25); border-radius: 8px;">
  <div style="font-weight: 600; margin-bottom: 0.5rem;">👀 Web & Visualization</div>
  <div style="display: flex; align-items: center; gap: 8px;">
    <img src="https://skillicons.dev/icons?i=fastapi,javascript,css,html,flask" height="32" alt="Web & Visualization" />
  </div>
</div>

</div>


## Education

[//]: # (GA Tech.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="gt_logo.png" alt="Georgia Institute of Technology" style="height: 30px; width: 30px; vertical-align: middle;">
    Georgia Institute of Technology
  </span>
  <span style="font-size: 16px; font-weight: 700;">Aug 2025～Dec 2026</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">M.Sc. Analytics
  @ <strong>School of Computational Science and Engineering</strong></span>
  <span style="font-size: 15px; font-style: italic;">Atlanta, Georgia</span>
</div>
<ul style="margin: 0 0 20px 1.2rem; padding: 0; font-size: 15px; line-height: 1.6;">
<br>

  | Category          | Courses                                                                         |
|-------------------|---------------------------------------------------------------------------------|
| **Agents & LLMs** | Conversational AI • Natural Language • Knowledge-Based AI                       |
| **DL / ML**       | Machine Learning • ML in Computational Biology                                  |
| **Frontend**      | Data & Visual Analytics • Principles-UI Software                                |
| **Statistics**    | Monte Carlo Methods • Nonparametric Data Analysis • Computing for Data Analysis |
| **Business**      | Business Analytics Practicum • Data Analytics in Business                       |
</ul>

<br>

[//]: # (UCSD.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="ucsd_logo.png" alt="University of California, San Diego" style="height: 30px; width: 30px; vertical-align: middle;">
    University of California, San Diego
  </span>
  <span style="font-size: 16px; font-weight: 700;">Sep 2019～Sep 2020</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">Master of Finance @ Rady School of Management</span>
  <span style="font-size: 15px; font-style: italic;">La Jolla, California</span>
</div>

<br>

[//]: # (NTU.)
<div class="exp-row" style="margin-bottom: 2px;">
  <span style="font-size: 18px; font-weight: 700; display: flex; align-items: center; gap: 8px;">
    <img src="ntu_logo.png" alt="National Taiwan University" style="height: 30px; width: 30px; vertical-align: middle;">
    National Taiwan University
  </span>
  <span style="font-size: 16px; font-weight: 700;">Sep 2014～Jun 2018</span>
</div>
<div class="exp-row" style="margin-bottom: 8px;">
  <span style="font-size: 15px; font-style: italic;">Department of Economics @ College of Social Sciences</span>
  <span style="font-size: 15px; font-style: italic;">Taipei, Taiwan</span>
</div>
