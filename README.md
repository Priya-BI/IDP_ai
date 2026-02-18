
# IDP "Intelligent Document Processing" #

 Its an automated extraction and structuring of information from documents using AI. 

[Difference between traditional and IDP] (image.png)

Function Used:
- ai_parse_document : Extract structured content from unstructured documents using a state-of-the-art generative AI model.

```
    create or replace temp view parsed_structured_doc as
    select path ,
    ai_parse_document(content) as parsed_content
    from raw_unstructured
```

- ai_extract : Extract entities specified by labels from text using a state-of-the-art generative AI model.
```
    select label, 
    ai_extract(table_html, array('CPT Code')).`CPT Code` as CPT_Code,
    ai_extract(table_html, array('ICD Code')).`ICD Code` as ICD_Code,
    ai_extract(table_html, array('Description')).`Description` as Description,
    ai_extract(table_html, array('Billed Amount')).`Billed Amount` as Billed_Amount,
    ai_extract(table_html, array('Paid Amount')).`Paid Amount` as Paid_Amount
    from structured_tables
```
  
- ai_classify : Classify input text according to labels you provide using a state-of-the-art generative AI model.
 ``` %sql
create or replace temp view parsed_classified_doc as
select *,
ai_classify(cast(parsed_content  as String), array('Invoice','Admin','Receipt','Purchase order')) as label
from parsed_structured_doc

 ``` 

Bills/invoice come in many formats and layouts — from different vendors, partners, and internal systems — making traditional rule-based parsing brittle and costly to maintain.

**This project shows how to leverage Databricks’ ai_parse_document together with Apache Spark to build a scalable, intelligent document processing pipeline that can adapt to varying document structures with minimal manual effort.**
  
### Goals: 
- Accept raw Medical bills (PDFs/images) from diverse sources
- Understand layout and content using AI-powered document parsing.
- Extract key business fields such as:
    - Claim id /paitent id
    - Date of service
    - Line Items (description, ICD code, CPT Code)
    - Total bill,Total paid.
- Convert unstructured documents into structured, analytics-ready tables.

### Steps to start
- Upload your PDFs to a Databricks Volume.
<img width="2012" height="800" alt="image" src="https://github.com/user-attachments/assets/09660586-cbe2-4c0f-b17f-8374f9766198" />

- Update the base_path variable in the notebook to point to your documents.
- Run the .ipynb notebook cells sequentially.
- View the extracted structured data in the output tables.
 <img width="2615" height="572" alt="image" src="https://github.com/user-attachments/assets/5c9d08a8-74b2-4726-b2d7-e81915438154" />



