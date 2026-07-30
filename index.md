# AI Storybook
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
|Anushka M | Suffern High School | Chemical or Biomedical Engineering | Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/LDwZ9YOMS1k?si=-OmTwEKo9Ekax1DY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my Bluestamp Engineering project, I'm creating an Interactive AI Storybook using Raspberry Pi and OpenAI. The project's goal is to allow the user to create personalized stories by speaking into a microphone. First, their prompt from their voice is converted into text using OpenAI's Whisper Speech Recognition Model. That text is then incorporated into a prompt that provides the AI with instructions about the story's format, length, and topic before being sent to ChatGPT, which generates a story based on the prompt. The completed story is then displayed on a touchscreen, making it similar to a kindle. The reason why I chose this project is because I already have experience with CAD design, fabrication, and robotics, but I wanted to challenge myself by learning new skills in electronics, embedded systems, and AI software integration. 

For my first milestone, I successfully created a working prototype capable of generating custom stories from a voice prompt. However, I ran into many challenges along the way. One of the biggest challenges I encountered involved the touchscreen ribbon cable. For example, while assembling the hardware, I was unaware that the connector latch needed to be opened before inserting the ribbon cable. This damaged both the ribbon cable and the connector. After that, I took the latches out of both connectors, which caused the one of my touchscreen to snap and the connector on my Raspberry Pi to be loose. After troubleshooting the issue, I replaced the damaged cable and properly reconnected the touchscreen. Another challenge I faced was with integrating OpenAi's API key. I initially believed the API key was free, which led to me facing issues while trying to execute the code. Due to this, I attempted to switch to Google's Gemini API, but encounted repeated 429 rate limit and compatability errors. After experimenting a little more, I switched back to OpenAI's API, added billing credits, and got my storybook working.

My next steps involve completing the physical assembly by mounting the Raspberry Pi to the touchscreen and placing the product inside the storybook. I also plan to expand on the project by adding NeoPixel LEDs and a magnetic sensor.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
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
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi 4 | Main processor that controls the entire AI storybook system | $149.79 | <a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1"> Link </a> |
| 7-Inch Touchscreen | Displays the storybook interface and generated stories | $38.99 | <a href="https://www.amazon.com/Hosyond-Touchscreen-Compatible-Capacitive-Driver-Free/dp/B0D3QB7X4Z/ref=sr_1_1?crid=ORF7VXSTLSE0&dib=eyJ2IjoiMSJ9.pOHOnV0W0xDkexguy-sSM18Ssx0wnu_V_lnRq_f9q-rmuaJPSXhF6hl2jHoMQIXxrm7zav5Nhra4FjLA_Qmvgah7JtXLaRLN8ky5Yapd_BK3PY9ffbX5po8l9fxBz9T5EUzqOHYomNxLX_Khg0yiUWfpjYFP4wV4o5M5a_RNFc7SksmXiYrKlTTEUu39PhkXqMlv97anFzH_2E0q-t79ieVGWU0hawNLVgl_aidXxg42btnpweKVkCJnygmqXyEx85f1SP4R9iSW3k18w7JjUeCAUdSJ4creTr2OgFnEq4c.P25tV5HW5mlBjdHQrDXZL7PGFo51qM3loPV3WmGdikQ&dib_tag=se&keywords=7%2Binch%2Braspberry%2Bpi&qid=1783981822&s=electronics&sprefix=7%2Binch%2Braspberry%2Bpi%2Celectronics%2C150&sr=1-1&th=1"> Link </a> |
| USB Microphone | Captures the user's voice for speech recognition and story generation | $7.96 | <a href="https://www.amazon.com/dp/B01MQ2AA0X?ref=fed_asin_title"> Link </a> |
| USB Mini Speaker | Plays generated stories aloud using text-to-speech | $13.99 | <a href="https://www.amazon.com/HONKYOB-Speaker-Computer-Multimedia-Notebook/dp/B075M7FHM1/ref=sr_1_5?dib=eyJ2IjoiMSJ9.6xySeP2KQ2waeO2PCFKgXydSBPn8zMGbV4f0PpDrivi1dSXmFsMVWaDtYKiBvAzMnpLJfOjboj8se6MFz1OV03KjbftCDQ-kTXDaa2axi2rkLhPa3au-lAEWRBmxKWs3p4GzpXMk6Fzp4ZXpevEsHVXieKvakfR7VCIyboe3c9MlDcxwdTCoy1EsykYr7CajrIe2NWaoY51ZzYqThDSe9-FTS8TuFBc0KwUG82fqJIs.FekNzrO6LkQMoaFuYFigpm0bYCCa5ARugXPSK08ndTs&dib_tag=se&keywords=speaker%2Bfor%2Braspberry%2Bpi&qid=1779543160&sr=8-5&th=1"> Link </a> |
| Magnetic Door Contact Sensor | Detects when the storybook is opened and activates the system | $9.99 | <a href="https://www.amazon.com/weideer-Magnetic-Surface-Normally-Contact/dp/B0BX2ZRZ8T/ref=sr_1_2?crid=31R9XWXYDU0UZ&dib=eyJ2IjoiMSJ9.KhkKOC7WxFoo6JJ2vLDRgHHS5My26-Ug6vT7JHDmUDpUdJ24EVPz0uUhN9c9M1ylM0h5E6WJL3RLjA3_KlvuPPV_VGXEZ7IQ9HYnXps2QlAN1F_2H-3hBPJ4txpSrvTe21uft2SdG1hbx_RFbKCMzlXla51-g54fcpFjhtyzh1Aqpxuy7JxVw6up1vIujE4muKh9FSsQ6crsIHOTY7aRqKcMzoAN0bcgrYNtwpvxpvk.5WVu68nz4HxkucEyWMLq2AVmZHIZQaDdAJJOZBmxgNw&dib_tag=se&keywords=l%2BEffect%2BSensor%2BDistributors%2BMagnetic%2Bcontact%2Bswitch%2Bbreadboard&qid=1749069523&sprefix=l%2Beffect%2Bsensor%2Bdistributors%2Bmagnetic%2Bcontact%2Bswitch%2Bbreadboard%2Caps%2C73&sr=8-2&th=1"> Link </a> |
| Fake Book Box | Physical storybook enclosure for electronics | $23.52 | <a href="https://www.amazon.com/dp/B0C4NKDWW6?lv=shuf&crid=12VYO3XQTCU1A&keywords=hollowed%20out%20book&sprefix=hollowed%20,aps,187&dib_tag=se&dib=eyJ2IjoiMSJ9.__udwxkKzpZUID8v1E5vEeuw0b7gPYz0TyNFwh5vPBCzVyYW8lnLBRLRzwpAz1Q1nON-7JrOfDpBIvXi92fCqy4JEkEeehv7uVKhMns2V9VQJL0D83FfZ6pOOoXFKYxS-9w8K6ZuaBubarzTSTM66lE1vU5rApFi8zfjGrM12NCmgUIfCAAJ1ungkIExPIsMfoP9EW-PNGeKF0CbfHSuOA2adoa9YEq4hBWZQqjJm7hcTUiOAV_RzcRAoDzl9L6MZ3U3OmHhgi9FFcoRPWd_xfjF_ebro2BdoyjF5ediedU.-oCXKxXKLSmnv7P1F3d4iNpSW_KlXwUZdvAb8G1Yjmg&qid=1750951553&sr=8-17&channelId=500&ref_=sr_1_17&plpRedirect=mhFallback"> Link </a> |
| Power Bank | Powers the storybook for portable operation | $32.99 | <a href="https://www.amazon.com/SIXTHGU-Portable-Charger-Charging-Flashlight/dp/B0C7PHKKNK/ref=sr_1_2_sspa?crid=2ZZM4AAZMMWHQ&dib=eyJ2IjoiMSJ9.W2Zx5_I3mKOn6UpwAzOw6PD0PNh1iaMRBiedequdv9weeWL0HPyPcxJBR9h6-LiFW-sHKnHSApN0sUxx0Q9xIRs80R57IlvvCsmEzXcktogo-4nP-NxrEZOy5dJTcXY8N-PBwfGt4fl_9LP8npenzDUV9TPA8KN6DMu175g6JegC_gZhAJrbqX94EfpQhLwP9vIJH45w2N-AFrfZZOy9jqk55gzVyk4Qst8uZvqn768.KBrc5_SqZ4e8zCpoFc-1C7rk02t3o2ykgDPB65W5JJU&dib_tag=se&keywords=always%2Bon%2Bpower%2Bbank&qid=1715957917&sprefix=always%2Bon%2Bpower%2Bbank%2Caps%2C107&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Digital Multimeter | Measures voltage and current during testing | $11.98 | <a href="https://www.amazon.com/gp/product/B0CXM242J1/ref=sw_img_1?smid=A34MWHFZRUFCUF&th=1"> Link </a> |
| Electronics Component Kit | Electronic parts for building and testing | $14.99 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1"> Link </a> |
| Screwdriver Set | Used to assemble and secure project hardware | $7.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
