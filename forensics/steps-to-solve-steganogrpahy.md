## Steganography
originally from https://bitvijays.github.io/LFC-Forensics.html. added modification to tailor team ususage. 
### Images
If you are looking for hidden flag in an image first check with
1. ```file, exiftool``` command, and make sure the extension is correctly displayed.
2. ```strings``` 
  2.1 Sometimes, it is better to see lines only greater than x length.
  ``` 
    strings RainingBlood.mp3 | awk 'length($0)>20' | sort -u
  ```  
3. ```binwalk``` the file, just to make sure, there’s nothing extra stored in that image.
4. ```hexdump -C``` and look for interesting pattern may be? If you get 7z or PK they represent Zipped files. If so, you can extract those file 
with 7z x . If somehow, you get a passphrase for the image, then you might have to use ```steghide``` tool as it allows to hide data with a passphrase.
5. ```stegsolve``` - check all the planes. There’s a data-extracter, we may try to extract all the values of RGB and see if there’s any flag in that.
6. ```stegosuite```
7. ```steghide``` : If there’s any text present in the Image file or the filename of the image or any link ( maybe to youtube video; video name can be 
the password ) that can be a passphrase to steghide. Sometimes, you may have to try all lowercase/ uppercase combinations.
8. ```zsteg``` : detect stegano-hidden data in PNG & BMP
9. ```pngcheck``` : pngcheck verifies the integrity of PNG, JNG and MNG files (by checking the internal 32-bit CRCs [checksums] and decompressing 
the image data); it can optionally dump almost all of the chunk-level information in the image in human-readable form.
10. ```Mediaextract``` : Extracts media files (AVI, Ogg, Wave, PNG, …) that are embedded within other files.
11. Comparing two similar images to find the difference
```        
    compare hint.png stego100.png -compose src diff.png
```
12. ```Image Arithmetic``` We can do image addition, subtraction, multiplication, division, blending, logical AND/NAND, logical OR/NOR, logical XOR/XNOR, Invert/ Logical NOT, Bitshift Operators.
13. We can use ```gmic``` to perform XOR of the images.
```
  gmic a.png b.png -blend xor -o result.png
```    
14. JPEG : ```Jstego``` : program aims at providing a java solution to hide secret information(such as secret file) to JPEG images. Hiding algorithm contains 
Jsteg and F5. The main(probably the toughest) stuff is encoding and decoding JFIF files.
15. JPEG : ```Jsteg``` : jsteg is a package for hiding data inside jpeg files, a technique known as steganography. This is accomplished by copying each bit 
of the data into the least-significant bits of the image. The amount of data that can be hidden depends on the filesize of the jpeg; it takes about 10-14 bytes 
of jpeg to store each byte of the hidden data.
16. Repair Corrupted JPEG/JPG, GIF, TIFF, BMP, PNG or RAW Image

### LSB Stegonagraphy
File are made of bytes. Each byte is composed of eight bits.
```   
10101100
1st digit is MSB and Last digit is LSB
```    
Changing the least-significant bit (LSB) doesn’t change the value very much.
```   
10101100(base 2) == 172 (10)
```
changing the LSB from 0 to 1:
```   
10101101(base 2) == 173 (10)
```   
So we can modify the LSB without changing the file noticeably. By doing so, we can hide a message inside.

### LSB Stegonagraphy in Images

LSB Stegonagraphy or Least Significant Bit Stegonagraphy is a method of stegonagraphy where data is recorded in the lowest bit of a byte.

Say an image has a pixel with an RGB value of (255, 255, 255), the bits of those RGB values will look like
```    
1 1 1 1 1 1 1 1
```   
By modifying the lowest, or least significant, bit, we can use the 1 bit space across every RGB value for every pixel to construct a message.
```    
1 1 1 1 1 1 1 0
```    
The reason stegonagraphy is hard to detect by sight is because a 1 bit difference in color is insignificant as seen below.
```       
Color 1    Color 2
FFFFFE     FFFFFF
```    
Decoding LSB steganography is exactly the same as encoding, but in reverse. For each byte, grab the LSB and add it to your decoded message. Once you’ve 
gone through each byte, convert all the LSBs you grabbed into text or a file.

### QRCodes?
Install ```zbarimg```     
```    
apt-get install zbar-tools
```     
Usage      
Read a QR-Code
```      
zbarimg <imagefile>
```      
Got a QR-Code in Binary 0101?, convert it into QR-Code by QR Code Generator

### Sound Files
1. Open the file in Audacity or Spectrum Analyzer and probably analyze the Spectogram
  1.1 Arrow next to the track name to switch from waveform (top) to logarithmic spectrogram (bottom).
  1.2 Morse code possible? As all the morse data appears to be below 100 Hz, we can use a low pass filter (effects menu, cutoff 100 Hz) to ease transcription
  1.3 Golang mp3 Frame Parser