# ChatGPT Magic Storybook
The ChatGPT Magic Storybook is a complex engineering project that is designed to allow users to generate a AI created fictional story based on a user input. The project uses the capabilities of the Raspberry Pi Software System to create a display that can be coded to take user input and be sent to ChatGPT to process using an API key. However, despite the project sounding easy, I faced many technincal difficulties throughout the project, including trouble oning the Raspberry Pi, my libraries not working, and the struggle to add a keyboard system. Still, despite that, I perservered through, working around my problems by using my resources and being patient.

| **Akshita S** | **Mountain House High School** | **Computer Science | CyberSecurity** | **Incoming Senior** |

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For my final milestone, I managed to get the keyboard to function, allowing me to complete my project. At first, I tried to create a keyboard using pygame however, when that didn't work, I decided to switch to the Vkeyboard library, which was more of a success. Eventually, once I got the keyboard button to function and added to enter button, I finished my ChatGPT Magic Storybook project. Despite the struggles I faced to complete my projects, the 3 weeks I spent in BlueStamp greatly helped me grow as a engineer and coder. I learned a variety of troubleshooting techniques, terminal commands, as well as basics of the Raspberry Pi system. Additionally, I learned about the importance of perseverance and patience when dealing with challenges that are a struggle to overcome. As I continue towards developing my career, I can't wait to continue applying and growing what I learned.


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/FI-mawsJNe4?si=_PC12QgC-3iuHHPE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second milestone, I managed to get the necessary python libraries working for my software code. Due to my systems's unability to find the libraries, I had to manually install each and every single library myself. IT WAS TIRING. At first, I tried Pip install however when that didn't work, I decided to switch to a better package manager: Anaconda. _However, _ that also didn't work due to Anaconda NOT supporting Raspberry Pi, making use switch to Anaconda's little brother, Miniconda. Once I installed Miniconda, I reinstalled all the libraries, but then we faced _another_ issue when installing the Python Speech Recognition library. To put to perspective, the Speech Recogniction library only supports python versions 10 and 11, but I had python version 13, making the systems incompatible. Therefore, we had to switch the user input from vocal input to keyboard input, changing a key part of my project. This modification gave way for the final step in the completion of my project.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/D3ggCFwkSOE?si=lFn47m6LgmDwdIsv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my first milestone, I connected my computer to the raspberry pi. In the span of four days, I faced many challenging rigours of pairing my Windows desktop to the Raspberry Pi operating system. On Monday, I was unable to install the Pi operating system from the Imager due to port incompatibility, resulting in me having to wait a day for an USB cable. Later, on Wednesday, when I install the pi, I tried to SSH my computer to the raspberry pi. However, that did not work. After spending 2 days troubleshooting the problem, we solved it once I connected the raspberry pi to the display. Once connected to the display, we found out that the raspberry pi was not connecting to my home internet, resulting in me having to manually connect it. Once the WiFi was connected, I was able to establish an SSH connection with my computer, solving my week worth long struggle. Due to this accomplishment, I was allowed to start the software implementation of my project. 

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```python
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| RasTech Raspberry Pi 4 Starter Kit | Used to install and connect Raspberry Pi | $103.99 | <a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1 "> Link </a> |
| Magnetic Door Switch | What the item is used for | $9.99 | <a href="https://www.amazon.com/weideer-Magnetic-Surface-Normally-Contact/dp/B0BX2ZRZ8T/ref=sr_1_2?crid=31R9XWXYDU0UZ&dib=eyJ2IjoiMSJ9.KhkKOC7WxFoo6JJ2vLDRgHHS5My26-Ug6vT7JHDmUDpUdJ24EVPz0uUhN9c9M1ylM0h5E6WJL3RLjA3_KlvuPPV_VGXEZ7IQ9HYnXps2QlAN1F_2H-3hBPJ4txpSrvTe21uft2SdG1hbx_RFbKCMzlXla51-g54fcpFjhtyzh1Aqpxuy7JxVw6up1vIujE4muKh9FSsQ6crsIHOTY7aRqKcMzoAN0bcgrYNtwpvxpvk.5WVu68nz4HxkucEyWMLq2AVmZHIZQaDdAJJOZBmxgNw&dib_tag=se&keywords=l%2BEffect%2BSensor%2BDistributors%2BMagnetic%2Bcontact%2Bswitch%2Bbreadboard&qid=1749069523&sprefix=l%2Beffect%2Bsensor%2Bdistributors%2Bmagnetic%2Bcontact%2Bswitch%2Bbreadboard%2Caps%2C73&sr=8-2&th=1 "> Link </a> |
| Precision Screwdriver | What the item is used for | $7.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/ "> Link </a> |
| Raspberry Pi Camera Module Case | What the item is used for | $8.99 | <a href="https://www.amazon.com/gp/product/B07RWCGX5K/ref=ox_sc_act_title_1?smid=A2IAB2RW3LLT8D&th=1 "> Link </a> |
| Mini Microphone | What the item is used for | $7.96 | <a href="https://www.amazon.com/dp/B01MQ2AA0X?ref=fed_asin_title "> Link </a> |
| Funny & STEM Bundle Electronic Components Kits | What the item is used for | $13.49 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1 "> Link </a> |
| Raspberry Pi 7" Touch Screen Display | What the item is used for | $85.92 | <a href="https://www.amazon.com/Raspberry-Pi-7-Touchscreen-Display/dp/B0153R2A9I/ref=sr_1_3?crid=19RRZQD9S67DT&dib=eyJ2IjoiMSJ9.aL1OKpRoz8hfrCyNs7-hTz4InmT2UBG-dTaTRP4wumB4WNHvjBR5eZYoZItPArRXdzGquf8578gGkqz1o4i3DLem3hDX96jPtO7V7-UJTX7q7CgVLrvhqz8KveRxHAuC89VlHWrIoAhJzWtZyu117vYjfIh9TjHpLg_ujzObFizoE5_B8aJhHH75ltLghA969edKiyymIfA2yaYjj3MkikD-eg-yg4ClX8okxsrkWIFho4LNlolQlTH5CPbZnMGicx-W6dPuHeZ3DuLGb3AyfOr-SJzQiRH8fj4H_TELe3A.MumpIeGcltRJ5GQyNFDl1XqAMIKqOSngK_vEvB5i2TM&dib_tag=se&keywords=7+inch+raspberry+pi&qid=1748991450&s=electronics&sprefix=7+inch+raspberry+pi%2Celectronics%2C139&sr=1-3 "> Link </a> |
| USB Adapter | What the item is used for | $4.99 | <a href="https://www.amazon.com/Reader-Adapter-Camera-Memory-Wansurs/dp/B0B9R7H765/ref=sr_1_2?crid=3MEQVHP531DE&dd=B-O1-WnNnW5Lq2sO8IIkLtNGzeC2M1AGI-JCm90QnS4%2C&ddc_refnmnt=free&dib=eyJ2IjoiMSJ9.krxmOj1H39tJVM38PBmRJnW35YzDDqIGdU3F97jHt6omRE5ZhWDvwdAi_2XUcK0j3Uyi_FqplJa-eGpzJgryaMhuG3C-O-yzKA-9Ug54I7La5wYvdMrU7wkPmx-arz1CKfttxKjBwfANjnsxaw0v9TEVBRBr64OwcLzGsKLCuHH5Jp8odkzjIc_8dSk7pv45Y4GSJ_XvYZ5ETaaXI5_nZXohd1MTGkr51XsGEOvQdNI.pZB6gFBJP2fqUzkGJDyCG3sR0G0Pj4hwswTH9WdB_CY&dib_tag=se&keywords=sd+to+usb+adapter&qid=1750106002&refinements=p_101%3A19346686011&rnid=19346684011&sprefix=sd+to+usb%2Caps%2C113&sr=8-2 "> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
