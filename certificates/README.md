# Creating HTML Certificate Templates for PDF Generation

This guide provides a comprehensive, beginner-friendly walkthrough on how to build robust, self-contained HTML templates designed specifically for conversion into PDF certificates.

## 1. The "Self-Contained" Rule
For a reliable PDF generation process, your HTML file must be a **single, independent unit**. This means it should not rely on external files, folders, or internet connections during the rendering process.

- **No External CSS:** All styles go inside `<style>` tags.
- **No External Images:** Images must be converted to **Base64 data strings**.
- **No External Fonts:** Use standard web-safe fonts or embed custom fonts using `@font-face` with Base64.
- **No JavaScript:** PDF engines usually render static content. Avoid JS for layout or data.

## 2. Document Structure & Sizing
Most certificates use **A4 Landscape** orientation. In CSS, we define the page size and ensure the body fills it perfectly.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <style>
        /* Define the page size and margins */
        @page {
            size: A4 landscape;
            margin: 0; /* Control margins inside the HTML instead */
        }

        body {
            margin: 0;
            padding: 0;
            width: 297mm;  /* A4 Width */
            height: 210mm; /* A4 Height */
            font-family: 'Georgia', serif;
            color: #333;
        }

        /* The main container that acts as the 'paper' */
        .certificate-container {
            position: relative;
            width: 100%;
            height: 100%;
            box-sizing: border-box;
            border: 20px solid #C5A059; /* Gold border example, not mandatory */
            background-color: #fff;
            display: table; /* Reliable centering for PDF engines */
        }
    </style>
</head>
<body>
    <div class="certificate-container">
    </div>
</body>
</html>

```

## 3. Handling Images (Base64)

You cannot use `<img src="logo.png">`. You must convert the image to a Base64 string.

**How to get a Base64 string:**

1. Use an online "Image to Base64" converter.
2. Copy the resulting long string of characters.
3. Paste it into your `src` attribute.

**Example:**

```html
<img src="data:image/png;base64,<base64-string-here>" />

```

## 4. Typography & Layout Tips

Since you are creating a document, layout behavior differs slightly from a website.

### Centering Text

The most reliable way to center content in PDF-rendering engines (like WeasyPrint or wkhtmltopdf) is using `text-align: center` on parent blocks or `display: table-cell`.

```css
.content {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}
```

### Positioning Elements

Use `position: absolute` for elements that need to be in specific spots, like a signature line in the bottom right or a seal in the corner.

```css
.seal {
    position: absolute;
    bottom: 50px;
    right: 50px;
    width: 100px;
}

```

## 5. Certificate Versioning

Versioning is a critical practice when managing automated certificates. It ensures that once a certificate is issued to a user, it remains consistent even if you decide to redesign your templates later.

### Why Versioning is Needed

* **Legal & Historical Accuracy:** A certificate is a formal record. If a student earned a certificate in 2023, they should be able to download that exact design in 2026, even if the company logo has changed since then.


* **Backend Identification:** The backend uses the version identifier to select the correct template logic and data fields required for that specific design.


* **Easy Recreation:** By storing the version identifier with the certificate record in the database, the system can perfectly recreate the original PDF at any time without guesswork.



### How to Handle Versioning

* **Naming Convention:** Versioning is handled via the file name. A common format is `Type_Version.html` (e.g., `participation_v1.0.html`, `participation_v2.0.html`).


* **Treat Old Versions as Archives:** Once a version is "live" and has been used to issue certificates, **do not alter it**. If you need to fix a typo or update a logo, create a new file (e.g., `v1.1`).


* **New Design = New Version:** Whenever a significant layout change or a change in required data fields occurs, increment the version number (e.g., `v1.0` -> `v2.0`) and create a new file.

* **Minor Updates:** When making adjustments to the existing latest design (most likely to fix a bug) increment the sub-version number (e.g., `v1.0` -> `v1.1`) and create a new file.

## 6. Step-by-Step Workflow

1. **Draft your layout:** Decide where the logo, recipient name, date, and signature will go.
2. **Prepare Assets:** Collect your logo and background patterns. Convert them all to Base64.
3. **HTML Setup:** Use the template structure provided in Section 2.
4. **Styling:** - Use `pt` (points) or `mm` (millimeters) for font sizes and margins instead of `px` (pixels). This ensures consistency on paper.
* `12pt` is standard for body text; `36pt`+ for titles.


5. **Dynamic Placeholders:** Use placeholders like `{{ .name }}` or `{{ .date }}` so the code can find and replace them easily, refer to the [Template Guide](../template_guide.md) for more details.

## 7. Common Pitfalls to Avoid

* **Avoid Flexbox/Grid:** While modern, some PDF engines have limited support for them. `display: block`, `inline-block`, and `table` are safer for legacy PDF tools.
* **Background Images:** If using a full-page background, set it on the `.certificate-container` using `background-image: url(data:...)`. Use `background-size: cover;`.
* **Colors:** Use CMYK-friendly HEX codes. Avoid overly bright "neon" colors that might look different when printed.

## 8. Example Certificate Content

```html
<div class="content">
    <h1 style="font-size: 50pt;">CERTIFICATE</h1>
    <p style="font-size: 18pt;">OF APPRECIATION</p>
    <br>
    <p>This is to certify that</p>
    <h2 style="font-size: 30pt; color: #C5A059;">{{ .Name }}</h2>
    <p>has successfully completed the workshop.</p>
</div>

```

*Created for beginners to master the art of HTML-to-PDF certificate generation.*
