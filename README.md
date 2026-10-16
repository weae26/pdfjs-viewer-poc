# pdfjs-viewer-poc

Authorized security testing PoC for PDF.js viewer `file=` parameter (hunan-bank.com, PDF.js 1.1.159, tested 2026-10-16).

## Files
- `pdf_xss_poc.pdf`: crafted PDF with 3 JS execution vectors, each attempting an OAST callback
  1. Catalog /OpenAction -> /S /JavaScript (callback paths /xss-openaction, /xss-oa2)
  2. Page /AA /O -> /S /JavaScript (callback paths /xss-pageopen, /xss-aa2)
  3. Type-4 PostScript calculator function with JS string via ShadingType 4 (callback path /xss-type4)

## Test result (2026-10-16)
Remote load viewer.html?file=https://raw.githubusercontent.com/weae26/pdfjs-viewer-poc/main/pdf_xss_poc.pdf
- PDF fetched (HTTPS, CORS ok), parsed, 1 page rendered: YES
- OAST callbacks from any of the 3 vectors: NO (0/3)

Conclusion: PDF.js 1.1.159 does NOT execute PDF-embedded JavaScript (OpenAction/AA unhandled; type-4 compiler is number-only, strings -> safe error). Remote PDF loading = phishing vector only, not XSS.