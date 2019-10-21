# BASH variable expension
for jpeg in `ls *.jpeg`;do mv $jpeg ${jpeg%.jpeg}.JPG;done
