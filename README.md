# PRD Genie — AI-Powered Product Documentation Assistant

### An Agentic AI Application Built with n8n, OpenAI LLMs, and a QA-Driven Evaluation Framework

PRD Genie is an AI-powered document generation assistant that transforms unstructured business requirements into structured product documentation.

The application accepts meeting transcripts, stakeholder notes, and product briefs, analyzes the information using Large Language Models (LLMs), and generates three key deliverables:

1. **Product Requirements Document (PRD)**
2. **User Stories with Requirement Traceability**
3. **Requirement Gap Analysis Report**

The solution is developed using **n8n workflow automation, OpenAI language models, JavaScript, and Google Drive integration**.

A dedicated evaluation framework has also been developed to assess requirement extraction, evidence grounding, contradiction detection, and generated document integrity.

---

## 1. Problem Statement

Product teams often receive requirements through multiple sources, including meeting discussions, stakeholder communications, and informal business documents.

Converting these inputs into structured product documentation can be time-consuming and may introduce several challenges:

- Missing or ambiguous requirements
- Contradictory stakeholder expectations
- Incomplete acceptance criteria
- Requirements without supporting evidence
- Inconsistent documentation
- Poor traceability between requirements and user stories

PRD Genie aims to reduce the manual effort involved in preparing initial documentation while identifying issues that require stakeholder clarification.

**Important:** AI-generated documentation is intended for human review and approval, not automatic acceptance as finalized business requirements.

---

## 2. Solution Overview

PRD Genie provides a browser-based document submission form powered by n8n.

### User Journey

1. The user opens the PRD Genie form using a browser URL.
2. The user uploads one or more supported `.txt` documents.
3. The application extracts and combines document content.
4. An AI Requirement Extractor identifies functional and non-functional requirements.
5. Grounding validation checks supporting evidence against the original documents.
6. The PRD Generator prepares structured product documentation.
7. The Story Breakdown component generates user stories.
8. The Gap Analyzer identifies missing, ambiguous, and contradictory requirements.
9. Three Markdown documents are generated and uploaded to Google Drive.
10. The completion screen displays links to the generated documents.

Users do not need access to the n8n workflow editor.

---

## 3. System Architecture

```text
                USER
                  |
                  v
           n8n Upload Form
                  |
                  v
        Normalize Uploaded Files
                  |
                  v
          Extract Document Text
                  |
                  v
            Combine Documents
                  |
                  v
        AI Requirement Extractor
                  |
                  v
       Validate Extracted Requirements
                  |
                  v
          Grounding Validation
             /          \
           PASS         FAIL
            |             |
            |       Evidence Repair
            |             |
            |         Revalidation
            |             |
            |       PASS / STOP
            v
          PRD Generator
                  |
                  v
         PRD Quality Validator
                  |
                  v
          Story Breakdown
                  |
                  v
             Gap Analyzer
                  |
                  v
         Compile Final Outputs
                  |
                  v
           Convert to Files
                  |
                  v
          Google Drive Upload
                  |
                  v
        Prepare Document Links
                  |
                  v
          Completion Screen
```

The workflow uses multiple specialized LLM processing stages with validation logic to improve requirement traceability and documentation quality.

---

## 4. AI Components

### 4.1 Requirement Extractor

Analyzes uploaded documents and extracts structured information, including:

- Functional requirements (FR)
- Non-functional requirements (NFR)
- Stakeholders and responsibilities
- Deadlines and dependencies
- Acceptance criteria
- Assumptions
- Ambiguous or unresolved items
- Contradictory requirements
- Supporting source evidence

Each extracted requirement is associated with a requirement ID and source reference.

### 4.2 Grounding Validator

Validates extracted requirements against uploaded source documents.

Checks include:

- Requirement ID format
- Duplicate requirement IDs
- Valid requirement status
- Source document references
- Supporting evidence availability
- Evidence quotation matching

The workflow also includes an evidence repair and revalidation path for certain validation failures.

### 4.3 PRD Generator

Converts structured requirements into a Markdown Product Requirements Document.

The PRD includes sections for product overview, objectives, functional and non-functional requirements, acceptance criteria, dependencies, assumptions, open questions, and timelines.

The generator is instructed not to invent unsupported requirements or present unresolved decisions as approved.

### 4.4 Story Breakdown

Transforms confirmed functional requirements into structured user stories.

Generated stories include:

- Epic and feature grouping
- User story identifiers
- User story descriptions
- Source requirement references
- AI-suggested priorities
- Acceptance criteria status
- Relevant non-functional constraints

### 4.5 Gap Analyzer

Analyzes extracted requirements, PRD content, and user stories to identify:

- Missing information
- Ambiguous requirements
- Contradictions
- Unresolved decisions
- Missing acceptance criteria
- Traceability gaps

The resulting report includes suggested clarification questions for stakeholders.

---

## 5. n8n Workflows

The project is organized into three workflows.

### Main Workflow

**File:** `workflow/prd-genie-main.json`

Responsible for the complete user-facing process, from document upload to Google Drive delivery.

### Processing Engine

**File:** `workflow/prd-genie-processing-engine.json`

A reusable document-processing workflow used by the evaluation framework to generate outputs from prepared test inputs.

### Evaluation Framework

**File:** `workflow/prd-genie-evaluation.json`

Executes predefined test scenarios and evaluates the generated results.

The evaluation workflow includes checks for requirement extraction behavior and document generation integrity.

**Note:** Workflow filenames should match the actual files committed to the repository.

---

## 6. QA-Driven AI Evaluation Framework

A key focus of this project is applying traditional QA principles to AI-generated outputs.

Unlike conventional software testing, LLM responses may vary between executions. Therefore, testing must evaluate not only whether the workflow executes successfully, but also whether the generated information is accurate, grounded, and traceable.

### Evaluation Scenarios

| Test ID | Scenario | Objective |
|---|---|---|
| T1 | Detailed Meeting Transcript | Verify extraction of explicit functional requirements, performance targets, stakeholders, and deadlines |
| T2 | Vague Product Brief | Verify that insufficient or ambiguous requirements are not converted into unsupported confirmed facts |
| T3 | Contradictory Requirements | Verify detection and reporting of conflicting requirements |

### Validation Dimensions

**Requirement Accuracy**

Checks whether extracted requirements preserve the meaning of source statements.

**Groundedness**

Checks whether generated requirements are supported by evidence in the uploaded documents.

**Traceability**

Checks whether generated requirements and user stories retain references to their sources.

**Contradiction Detection**

Evaluates whether conflicting statements are identified rather than silently resolved.

**Document Integrity**

Checks whether the expected PRD, User Stories, and Gap Analysis outputs are generated.

**Negative Testing**

Uses vague and contradictory inputs to evaluate the application's behavior when requirements are incomplete or inconsistent.

### Evaluation Status

The evaluation framework has been exercised using T1, T2, and T3.

During development, several failures were identified and investigated, including evidence mismatch, missing document outputs, and contradiction-detection failures.

Document-generation checks were subsequently improved.

Some evaluation behavior, particularly contradiction detection, requires further verification. Additional planned test cases remain outside the current validated scope.

**A successful workflow execution does not automatically mean that the AI-generated content is correct.**

---

## 7. Technology Stack

| Technology | Purpose |
|---|---|
| n8n Cloud | Workflow orchestration and form-based user interface |
| OpenAI GPT Models | Requirement extraction and document generation |
| Structured Output Parser | Structured requirement extraction |
| JavaScript | Input transformation, validation, evaluation, and output preparation |
| Google Drive | Generated document storage and delivery |
| Markdown | PRD, User Stories, and Gap Analysis output format |
| GitHub | Source control, documentation, and project portfolio |

---

## 8. Project Features

- Browser-based document upload
- Multiple source-document processing
- AI-powered requirement extraction
- Functional and non-functional requirement identification
- Source evidence validation
- Requirement traceability
- Evidence repair and revalidation
- Structured PRD generation
- User story generation
- Gap analysis and stakeholder clarification questions
- Google Drive integration
- Clickable document links
- Dedicated AI evaluation workflow
- Positive and negative test scenarios

---

## 9. How to Run the Project

### Prerequisites

- n8n environment
- Access to compatible OpenAI models
- Google Drive account with available storage
- Required n8n credentials and permissions

### Setup

1. Import the Main Workflow JSON into n8n.
2. Configure the OpenAI model credentials.
3. Configure the Google Drive OAuth connection.
4. Review the Google Drive upload destination.
5. Save and publish the workflow.
6. Open the Form Trigger Production URL.
7. Upload supported `.txt` requirement documents.
8. Submit the form and wait for processing.
9. Open the generated document links from the completion screen.

The Processing Engine and Evaluation Framework can be imported separately for evaluation.

Workflow exports do not include usable authentication secrets. Credentials must be configured in the target n8n environment.

---

## 10. Testing and Current Project Status

### Verified Functionality

- Form-based document submission
- AI document generation
- Creation of three Markdown outputs
- Google Drive file uploads during successful test executions
- Dynamic generation of document links
- Clickable links on the completion screen
- Production form accessibility without n8n login

### Known Issue

During a production end-to-end test, the Google Drive upload node returned a storage quota exceeded error.

This prevented document delivery for that execution.

**Resolution:** Free up storage or configure another Google Drive account with available capacity, then rerun the production test.

### Current Status

**Functional prototype with additional production validation pending.**

The core workflow has demonstrated successful end-to-end execution. Production delivery requires retesting after the storage issue is resolved.

---

## 11. Future Enhancements

Potential improvements include:

- Expanded automated AI evaluation test coverage
- More robust contradiction detection
- Additional negative and boundary test scenarios
- Automated regression evaluation
- Improved error handling and user-facing failure messages
- Additional document format support
- Dedicated frontend dashboard
- Secure document access controls
- Document history and version management
- Evaluation reports and quality metrics

---

## 12. QA and Quality Engineering Perspective

This project demonstrates how traditional QA practices can be extended to AI-powered applications.

Key areas explored include:

- Requirement validation
- Positive and negative testing
- AI hallucination and grounding checks
- Source traceability
- Data validation
- Workflow integration testing
- Document output validation
- Defect investigation
- Production user acceptance testing

The objective is not only to generate documents using AI, but also to develop a systematic approach for evaluating their reliability.

---

## 13. Project Purpose

Developed as part of the **Interview Kickstart Agentic AI learning program**, PRD Genie is a hands-on project exploring AI workflow orchestration, LLM-based document processing, and quality engineering practices.

The project combines experience in software quality assurance with practical exposure to AI application development and evaluation.

---

## 14. Disclaimer

PRD Genie is a learning and demonstration project.

AI-generated documents should be reviewed by qualified stakeholders before being used for business, development, or delivery decisions.

Do not upload confidential customer information, production credentials, or proprietary business documents to a publicly shared demonstration environment without appropriate authorization.
