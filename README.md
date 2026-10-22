# CS 305: Artemis Financial Practices for Secure Software

This repository contains the software security portfolio artifact and comprehensive security report developed for **Artemis Financial** as part of CS 305: Software Security. The primary artifact included is the complete **Practices for Secure Software Report** (`CS 305 Project Two_Jungclas.docx`), documenting the vulnerability assessment, transport layer security hardening, cryptographic implementation, and static security analysis conducted on Artemis Financial's web service.

---

## Module Eight Portfolio Reflection & Journal Responses

### 1. Client Summary and Software Requirements
* **Client Overview:** Artemis Financial is a financial consulting and wealth management firm specializing in customized financial planning, including investments, retirement plans, tax strategies, and savings.
* **Problem Statement:** The company sought to modernize its digital operations by introducing web-based services. However, their existing software prototype lacked essential cybersecurity controls, transmitting data over unencrypted cleartext channels without mechanism to verify data integrity or authenticate server communications.
* **Core Requirements:** Artemis Financial engaged Global Rain to conduct a comprehensive security review and refactor their Spring Boot application. The key technical mandates required:
  * Eliminating cleartext communications by enforcing Transport Layer Security (TLS/HTTPS).
  * Binding authenticated identity through an X.509 cryptographic certificate.
  * Implementing a cryptographic checksum verification endpoint to confirm the authenticity and integrity of financial plan transfers.
  * Ensuring zero regressions or newly introduced third-party vulnerabilities through automated security testing.

---

### 2. Secure Coding Execution and Business Value
* **Execution Strengths:** When evaluating the codebase, I successfully identified gaps across syntactical, logical, and structural security tiers. Rather than merely applying a superficial fix, I implemented standard-compliant Java Cryptography Architecture (JCA) components, eliminated insecure defaults, configured TLS termination on port 8443, and tied server trust to an authenticated Java KeyStore (`keystore.jks`).
* **Importance of Secure Coding:** Software security is foundational rather than an auxiliary feature. Flaws introduced at the architecture or code level leave applications vulnerable to credential theft, eavesdropping, and Man-in-the-Middle (MITM) attacks. Writing defensive code with least privilege and strong typing prevents vulnerabilities before deployment.
* **Organizational Value:** For a financial consulting institution, software security directly preserves institutional viability. Implementing industry-standard security safeguards nonpublic personal financial information (NPI), mitigates severe financial and litigation liabilities, ensures compliance with statutory mandates (GLBA Safeguards Rule 16 C.F.R. § 314.4(c)(4), SOX, FIPS 140-3), and maintains vital client trust.

---

### 3. Vulnerability Assessment: Challenges and Takeaways
* **Helpful Aspects:** The OWASP Dependency-Check static analysis tool provided clear visibility into transitive dependencies and vulnerable third-party libraries across the Maven dependency tree. Mapping real-world dependencies to published CVEs demonstrated how quickly third-party code can expand an application's attack surface.
* **Challenging Aspects:** The most challenging element was diagnosing environment and configuration issues during secondary scanning and server deployment—specifically resolving NVD API connectivity limits, disabling unnecessary commercial analyzers (e.g., OSS Index), and diagnosing runtime port conflicts (address-in-use on port 8443) caused by lingering background processes. Resolving these challenges deepened my understanding of embedded servlet container lifecycles and build-chain configurations.

---

### 4. Defense-in-Depth and Future Assessment Strategies
* **Layered Security Implementation:** A four-tier defense-in-depth model was deployed across the application architecture:
  * *Layer 1 (Identity & Trust):* Generated a 2048-bit RSA key pair and self-signed X.509 certificate via Java `keytool` to authenticate server identity and establish domain trust.
  * *Layer 2 (Transport Encryption):* Refactored `application.properties` to enforce HTTPS over port 8443, mitigating packet sniffing and MITM risks.
  * *Layer 3 (Cryptographic Data Integrity):* Implemented a `/hash` REST endpoint executing native SHA-256 digests over UTF-8 byte arrays, providing deterministic 64-character verification to detect data alteration.
  * *Layer 4 (Software Supply Chain Governance):* Utilized continuous static dependency scanning via OWASP Dependency-Check to monitor third-party frameworks against known vulnerabilities.
* **Future Strategies:** In future development, I will adopt a "Shift-Left" security strategy. I will incorporate automated Static Application Security Testing (SAST) directly into CI/CD build pipelines, apply Software Bill of Materials (SBOM) generation, leverage NIST SP 800-53 and OWASP Top 10 guidelines for threat modeling, and conduct structured manual code reviews before committing code to production.

---

### 5. Functional and Security Verification
* **Functional Execution:** Functional correctness was validated in the Eclipse IDE. The refactored `SslServerApplication.java` compiled cleanly, bound the `keystore.jks` certificate to the embedded Tomcat container, and launched on port 8443 without runtime errors or exceptions. The `/hash` route successfully rendered the SHA-256 digest (`1a5615af...f190`) alongside the custom verification string in a web browser.
* **Regression and Vulnerability Checks:** Following refactoring, I executed a secondary static analysis build using the OWASP Dependency-Check Maven plugin (`v12.1.0`). The scan completed with a `BUILD SUCCESS` status and confirmed that the functional additions (utilizing native `java.security.MessageDigest` and `java.nio.charset.StandardCharsets`) relied solely on built-in Java runtime packages, introducing zero new external libraries or security vulnerabilities to the dependency baseline.

---

### 6. Tools, Practices, and Reusable Resources
The workflows, tools, and technical practices mastered during this project will serve as permanent assets in future engineering roles:
* **Tooling:** Java `keytool` for cryptographic certificate and keystore management; Maven for build automation; OWASP Dependency-Check for Software Composition Analysis (SCA); and modern IDEs (Eclipse) for JVM runtime debugging.
* **Cryptographic Standards:** Practical experience configuring standard-compliant algorithms (AES-256-GCM, SHA-256, RSA 2048-bit), enforcing explicit UTF-8 character encoding, and managing X.509 certificate lifecycles.
* **Secure Engineering Principles:** Applying defensive exception handling (`NoSuchAlgorithmException`), principle of least privilege, attack surface reduction, and automated dependency scanning.

---

### 7. Portfolio Showcase for Future Employers
This artifact serves as tangible evidence of full-lifecycle secure software engineering skills:
* **Practices for Secure Software Report:** An executive-ready technical document detailing cryptographic rationale, regulatory alignment, terminal commands, configuration refactoring, and static analysis interpretation.
* **Demonstrated Technical Competencies:**
  * Proficiency in identifying and remediating syntactical, logical, and architectural security vulnerabilities.
  * Hands-on implementation of enterprise TLS/HTTPS and X.509 keystore integration within a Spring Boot microservice.
  * Rigorous cryptographic implementation using Java Cryptography Architecture (JCA).
  * Automated security governance using industry-standard SCA vulnerability tooling.

---

## Repository Contents
* `Practices for Secure Software Report.pdf`: Complete, formatted technical report submitted for Project Two, containing full vulnerability reviews, configuration architectures, and verification screenshots.
* `README.md`: Module Eight Journal reflection and technical overview.
