Here are some regex exercises:
1. Match a string consisting entirely of digits:
/^\d+$/
2. Match a string with only alphanumeric characters and with length at least 4.
/^\w{4,}/
3. Match a string containing two words that are the same:
/b\[a-zA-Z].+b\1\b/
4. Match a string containing 4 consecutive lowercase consonants:
/[b-df-hj-np-tv-z]{4}+./
5. Match a person's name, for example Harry Potter. The name is
assumed to consist of two words, each starting with a capital letter and consisting entirely of letters (from the usual alphabet, without accents). There may be one or more white space characters separating the two parts.
// /^[A-Z][a-z]+/ /+[A-Z][a-z]+/
6. Match a valid postal code, for example M5R 1E4 (with one space in between):
/[A-Z][0-9][A-Z] ?[0-9][A-Z][0-9]/
7. Match a valid postal code with all digits the same, for example M5R 5E5:
/[A-Z]([0-9])[A-Z] ?\1[A-B]1\$/
8. Match a string that represents time in HH:mm:ss format. HH is hours between 01 and 12. mm and ss represent minutes and seconds, and each is
between 00 and 59. Valid examples include 01:26:18 and 11:00:39. Invalid examples include 13:01:01, 00:00:00, and 01:00:60v:
^(0[1-9]|1[0-2]):([0-5][0-9]):([0-5][0-9])$

9. Consider this file format:
ANA300Y1<T>Y<T>Human Anat Histol <T>LEC<T>0101<T>0350<T>0325<T>000
ANA301H1<T>S<T>Human Embryology <T>LEC<T>0101<T>0500<T>0500<T>054
ANA498Y1<T>Y<T>Research Project <T>LEC<T>0101<T>9999<T>0012<T>000
ANT100Y1<T>Y<T>Intro Anthropology <T>LEC<T>5101<T>1250<T>1129<T>000
ANT200Y1<T>Y<T>Intro Archaeology <T>LEC<T>5101<T>0300<T>0281<T>000
CSC148H1<T>S<T>Intro to Comp Sci <T>LEC<T>0102<T>0100<T>0061<T>000
CSC148H1<T>S<T>Intro to Comp Sci <T>LEC<T>5101<T>0120<T>0117<T>000
CSC148H1<T>S<T>Intro to Comp Sci <T>TUT<T>0101<T>0060<T>0060<T>000
CSC148H1<T>S<T>Intro to Comp Sci <T>TUT<T>0201<T>0048<T>0019<T>000
CSC207H1<T>F<T>Software Design <T>LEC<T>0101<T>0120<T>0096<T>000
CSC207H1<T>F<T>Software Design <T>LEC<T>5101<T>0085<T>0043<T>000
Lecture Term Course title LEC/TUT SEC CAP ENRLMT WAITING_LIST You can assume that the number of characters before the first tab character and between successive tab characters is always the same in

every line of the file. (a) Write a regular expression to match the lecture ("LEC") sections of fall-term courses with names beginning "Intro". Group the course codes ("XXX123..") and section numbers (for example, 5203) so that they can be extracted from the matched substring. (b) The third-to-last and second-to-last columns indicate the enrolment cap and the actual enrolment. Write a regular expression to match every line where the enrolment is at the cap. (c) Write a regular expression to match any line where the course title is shorter than 10 characters, not including trailing blanks. For example, the course title for CSC148H1 is "Intro to Comp Sci". There is no need to group any part of the string, though you may do so if you find it helps. (There are 20 characters in the course title area of each line. At least the first character of the title must be non-blank.)