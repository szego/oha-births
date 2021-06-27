
# Processing OHA's Final Method of Delivery Data

The Oregon Health Authority (OHA) publishes [statistics on births in Oregon by county](https://www.oregon.gov/oha/PH/BIRTHDEATHCERTIFICATES/VITALSTATISTICS/BIRTH/Pages/index.aspx) each year in PDF format. The goal of this project is to programmatically read and process all of their "Final Method of Delivery by Facility" PDFs into a format that can be easily analyzed using standard statistical software.

The final CSV file containing all of the data in OHA's PDFs can be found in the `\output\` folder.

It is not necessary to run any of the R scripts in this project if all you need is the data - the CSV can be used as a standalone data source. The R scripts used to create the CSV are only provided for documentation purposes to show how the PDFs were processed.

## Details on OHA's PDFs

The PDFs published by OHA contain counts of births, in total, and by the method in which the baby was delivered: vaginal, vaginal after a previous cesarean section (VBAC), and cesarean section. These counts are provided at the state level, county levels, and within each county by the facility at which the birth occurred.

The original PDFs can be found in the `\data\` folder.

While the general shape of the content has remained mostly the same, its precise formatting (spacing, font sizes, etc.) has changed several times since the first PDF was published in 2008. This makes it difficult to process all of the documents using the same algorithm.

Here's what the top of the 2008 PDF looks like:
![Image of OHA's 2008 PDF](/doc/readme_imgs/pdf_08.png)

The formatting was mostly unchanged until 2013, when OHA changed the words it capitalized:
![Image of OHA's 2013 PDF](/doc/readme_imgs/pdf_13.png)

OHA used this formatting (but with different footnotes from year to year) until 2017, when it added more lines to the title:
![Image of OHA's 2017 PDF](/doc/readme_imgs/pdf_17.png)

In 2018, the superscript for the footnote on "VBAC" was the same font size as the word itself:
![Image of OHA's 2018 PDF](/doc/readme_imgs/pdf_18.png)

Lots changed in 2019, including font sizes and vertical and horizontal spacing:
![Image of OHA's 2019 PDF](/doc/readme_imgs/pdf_19.png)

The 2020 PDF's formatting was visually very similar to the 2019 PDF's, but it used a slightly different spacing scheme to achieve it. (Picture not included since the difference wouldn't be visible.)

One other notable formatting quirk, present in multiple years, is that some facility names are broken into multiple lines:
![Image of a multiline facility name](/doc/readme_imgs/multiline_facility_name.png)
Note that the trailing dots and numerical counts only appear on the second line for that facility.

All of these formatting changes posed challenges to writing a single algorithm that would work for all of the documents. The earliest version of the algorithm was written for the 2020 PDF, and the changes to the spacing scheme caused it to fail for the 2019 PDF. The next version, which worked for the 2020 and 2019 PDFs, could not read the 2018 PDF at all, so I had to find a different method to detect the county headings.

In the end I was able to develop an algorithm that worked for all 13 years of data. Assuming OHA keeps the formatting more or less the same, this code should continue to work on new PDFs published in the coming years. It is a bit fragile, though, and OHA will no doubt tweak the formatting again, so it will probably require a fix eventually.

-----

Project created on 2021-04-13 by Antonio R. Vargas.

Folder legend:

- analysis: RMarkdown files that constitute the final data processing or analysis

- src: R scripts that contain useful helper functions or other set-up tasks (e.g. data pulls)

- data: Raw data - this folder should be considered read only!

- output: Intermediate data objects created in the analysis

- doc: Any long form documentation or set-up instructions

- ext: Any miscellaneous external files or presentation material collected or created throughout the analysis

