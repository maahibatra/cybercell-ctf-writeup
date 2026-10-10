# CyberCell CTF Write-up

This repository documents my solutions to the six challenges in the COEP CyberCell FY Inductions 2026 Mini CTF. Each write-up covers the approach I used to find the flag, along with the relevant concepts behind the solution.

The flags themselves have been omitted to avoid directly disclosing the answers.

---

## Challenge 1 — Nothing to See Here

**Category:** Web Security

The webpage claims that everything has been scrubbed before deployment, and there is no flag visible on the page.

However, the browser receives more than just the content it renders on screen. Inspecting the page's HTML source revealed a comment that the developers had left behind, containing the first flag.

**Key takeaway:** HTML comments are not displayed on the webpage, but they remain accessible in the source. Anything included in publicly accessible client-side code should be treated as visible to users.

## Challenge 2 — Mystery String

**Category:** Encoding

The challenge presents an alphanumeric string described as an "intercepted transmission payload". Its format suggested Base64 encoding, although the usual `=` padding was missing.

Since Base64 padding is optional in some contexts, I tried decoding the string using CyberChef's Base64 decoder. The output revealed the second flag.

**Key takeaway:** Recognising common encoding patterns helps narrow down possible solutions. Encoding is not encryption, and Base64 does not provide confidentiality.

## Challenge 3 — Picture Perfect

**Category:** Digital Forensics

The challenge provides an image with no obvious information visible in its contents. This suggested that the file itself might contain something worth investigating.

I downloaded the image and used `exiftool` to examine its metadata. The output contained the third flag.

```bash
exiftool <image-filename>
```

**Key takeaway:** Image files can contain metadata beyond their visible contents. Examining this metadata is a useful first step when investigating files for hidden or overlooked information.

## Challenge 4 — Polite Robots

**Category:** Web Security

A website's `robots.txt` file specifies paths that web crawlers are requested not to access. These directives are not security restrictions, however, and the paths themselves may still be publicly accessible.

Checking the file revealed a disallowed path, `/s3cr3t-vault-7f3a/`. I navigated directly to that path, where the webpage disclosed the fourth flag.

**Key takeaway:** `robots.txt` should not be used to protect sensitive resources. If a page needs to be restricted, access must be enforced by the server.

## Challenge 5 — Where Was This Taken?

**Category:** OSINT

OSINT (Open-Source Intelligence) involves gathering and analysing publicly available information to identify people, places, or other relevant details.

Even when an image's GPS coordinates and camera metadata have been removed, visual details can still provide clues about its origin.

I examined the image for recognisable features and used reverse image search to narrow down the location. These clues led me to COEP's Main Building, which was the answer to the challenge.

**Key takeaway:** Removing metadata does not necessarily prevent geolocation. Landmarks, architecture, signs, and other visual features can reveal where an image was taken.

## Challenge 6 — Not What It Seems

**Category:** Digital Forensics

The challenge provides a file named `mystery.jpg`, supposedly an image left behind by a suspect. However, the file would not open as an image, suggesting that its extension might be misleading.

I downloaded the file and checked its actual format using the `file` command:

```bash
file mystery.jpg
```

The output identified it as a ZIP archive rather than a JPEG. I renamed it to `mystery.zip` and extracted its contents, finding a text file containing the final flag.

**Key takeaway:** File extensions can be misleading. Identifying a file by its actual format, rather than its name alone, helps determine how to inspect it correctly.

---

## Conclusion

The six challenges introduced several practical techniques, including inspecting HTML source code, decoding Base64, examining image metadata, checking `robots.txt`, using OSINT for geolocation, and identifying file formats.

The common thread was looking beyond what was immediately visible: information that appeared absent or inaccessible often became straightforward to find once I examined the underlying data.
