# PRD Genie — AI-Powered Requirements Documentation Assistant

PRD Genie is an **n8n-based prototype** that transforms uploaded requirement documents into a draft Product Requirements Document (PRD), user stories, and a requirements gap analysis.

## What it does

1. A user opens an n8n-hosted form and uploads one or more `.txt` documents (meeting transcript, stakeholder notes, or product brief).
2. The workflow extracts and combines the source text, retaining source IDs and filenames.
3. An LLM extracts structured functional/non-functional requirements, stakeholders, timelines, ambiguities, and contradictions.
4. Code-based checks validate requirement IDs and supporting evidence; an evidence-repair/revalidation path handles some mismatches.
5. LLM steps generate a Markdown PRD, user stories, and a gap analysis report.
6. The workflow converts the three reports to files, uploads them to Google Drive, and shows clickable links on the completion screen.

## Workflow

`Form Trigger → Normalize Files → Extract Text → Combine Documents → Requirement Extractor → Grounding Validation / Repair → PRD Generator → PRD Quality Validator → Story Breakdown → Gap Analyzer → Compile Outputs → Convert to File → Google Drive Upload → Prepare Links → Form Ending`

The export also contains a separate manual-trigger sample-data path used during development.

## Technology

- n8n workflow automation and hosted forms
- OpenAI models via n8n AI nodes (model availability and credentials depend on the importing instance)
- JavaScript Code nodes for extraction support, validation, and output assembly
- Google Drive for output storage
- Markdown for generated reports

## Getting started

1. Import `workflows/prd-genie-main.json` into your own n8n instance.
2. Configure the OpenAI model connections and Google Drive OAuth credentials in n8n. **No credentials are included in this repository.**
3. Review the Form Trigger fields, file-size limits, and Google Drive destination/permissions.
4. Test with fictional `.txt` input documents before publishing the workflow.
5. Publish the workflow and use its **Production Form URL** for user submissions. Do not publish private form URLs or private output links in the repository.

## Validation and project status

- The core form-to-document workflow and clickable completion links have been demonstrated in testing.
- The production form was confirmed accessible without an n8n editor login.
- A production end-to-end test was blocked at the Google Drive upload node because the connected account exceeded its storage quota. **Production end-to-end UAT remains pending after storage remediation.**
- A separate evaluation framework was developed, but is **not included** in this main-workflow export; full evaluation coverage and final acceptance have not been claimed.
- Generated AI content is a **draft** and requires human review. Evidence checks and section-presence checks do not guarantee semantic accuracy or completeness.

## Security and limitations

- Uploaded documents may be sent to the configured AI provider. Do not use confidential client data without appropriate authorization.
- Access to the form and Google Drive links must be managed independently. A clickable link does not automatically grant access to a file.
- The upload form currently accepts `.txt` files; PDF and DOCX input are not implemented in this export.
- Google Drive storage availability is an external dependency; the current workflow does not implement a fallback delivery mechanism.
- The workflow JSON is a sanitized configuration template, not a self-contained application. Importers must configure their own credentials.

## Portfolio relevance

This project demonstrates **AI-assisted requirements analysis, grounding/evidence validation, document-generation workflows, negative-testing considerations, and end-to-end QA thinking**. It is a prototype, not a production-certified document authoring system.
