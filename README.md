# Resume

I discute about the color quantization problem a try to solve it, approximately, using the GRASP meta-heuristics. The .ipynb file has the implementation and some examples of its use. The "color quantization" part are more mathematical, so you can skip it if you want. At the end I show some results of my implementation.

# Color quantization

The one of the mostly image format used in computers and other devices is the RGB format. In this format there's three channels which the values can vary between 0 and 255. The pixels, therefore, are represented by three values, one for each channel. So we have $256^{3}$ possible colors! That's a lot more the number of colors a human can recognize. In fact, all this huge number of possible colors can enlarge the image file size. So what if we delimit the number of colors of our image? Is it possible to reduce drastically this number and maintain a good image quality? Yes!

Color quantization looks to solve this problem: To find a good color palette that reduce the number of colors of the image and preserve its quality. There's a lot of ways to define this problem more precisely and a lot of algorithms to solve it. I'll define it in a specific way, but remind that it's the unique way to do that. First let's remind that in the RGB format images with $m$ pixels by $n$ pixels can be represented as $3$ matrices with $m \times n$ dimension, one matrix for each channel containing the value of the pixels in that channel.

So let $I$ be the $3 \times m \times n$ matrix representation of the original image. I want $\hat{I}$ such that it minimizes

$$f(\hat{I}) = |I - \hat{I}|_{F}^{2}$$

Where $|\cdot|_{F}$ is the Frobenius norm with definition

$$|A_{m \times n \times o}|_{F}^{2} = \sum\_{i=1}^{m}\sum\_{j=1}^{n}\sum\_{k=1}^{o}a\_{ijk}^{2}$$

Frobenius norm tries to generalize the vector norm. So the intuition is that we want a matrix $\hat{I}$ that's "close" the $I$ matrix. Note that the "palette" doesn't appear hear. But we can make it happen. Let's define the function that recolor the image with some palette $P$ of length $k$ as

$$p_{k}(I, P)\_{:ij} = arg\min_{l}\|I_{:ij} - P_{l}\|$$

That is, the color in the palette that's close to the pixel will be the new color of that pixel. So we have $f$ in function of $P$ as

$$f(P)=\|I - p_{k}(I, P)\|_{F}^{2}$$

And that's what we'll minimize. Unfortunely, this is a NP-Complete problem. If we have $256^{3}$ possible colors, how many palettes of length $k$ can we have? A lot of them! More precisely, we have

$${256^3 \choose k} = \frac{(256^{3})!}{k!(256^3 - k)!}$$

Choices. So, to define what palette is optimum is impractical. For solve this, I'm going to use the GRASP meta-heuristic.

# Results

A tested this implementation in some images.

## Naruto image

![time7](https://github.com/user-attachments/assets/08821da0-1110-4df8-9f0e-b4dff575921b)

First I try to reduce the number of colors to $16$. The other parameters I used was: size of the neighborhood $= 100$, changes in the palette $= 4$ and number of iterations $=60$. The results are shown below.

### Image obtained by GRASP in iteration 1:

![time7_1](https://github.com/user-attachments/assets/a86e7397-3247-4f54-b040-115671e93e32)

### Image obtained by GRASP in iteration 2:

![time7_2](https://github.com/user-attachments/assets/70a88e66-91cd-48c7-952f-de3c218407a7)

### Image obtained by GRASP in iteration 4:

![time7_4](https://github.com/user-attachments/assets/5f885c9f-3c94-46c0-a8f4-477ab747ffeb)

### Image obtained by GRASP in iteration 10:

![time7_10](https://github.com/user-attachments/assets/8ee7b908-74f1-4e58-befa-a79b8a4b441b)

### Image obtained by GRASP in iteration 60:

![time7_60](https://github.com/user-attachments/assets/75ea3e74-0bea-41f3-b783-d788074b76b3)

I also tried to reduce to $64$ colors. In this test I used $16$ changes in palette, $100$ neighbours and $60$ iterations. The results got better! And that makes sense, since the model have more liberty to choose colors to paint the image.

### With $64$ colors:

### Image obtained by GRASP in iteration 1:

![time7_64_1](https://github.com/user-attachments/assets/8f8aa983-1692-49cc-a56a-596f3deb4678)

### Image obtained by GRASP in iteration 2:

![time7_64_2](https://github.com/user-attachments/assets/3104803a-2355-40de-b9c2-91ff2c536928)

### Image obtained by GRASP in iteration 3:

![time7_64_3](https://github.com/user-attachments/assets/f234db1e-e875-404a-a4c2-91a49ed91894)

### Image obtained by GRASP in iteration 7:

![time7_64_7](https://github.com/user-attachments/assets/c401a635-8c49-4b88-983f-37c4951083cf)

### Image obtained by GRASP in iteration 25:

![time7_64_25](https://github.com/user-attachments/assets/f41ce77c-af9f-47a1-9402-d3592e7bb395)

## Pokémon Yellow image

![pokemon_yellow](https://github.com/user-attachments/assets/00f4fca8-7fba-486d-a340-b61e5fdc99d8)

Pokémon Yellow was a 90s' game released in GameBoy Color (GBC) and other Nintendo's console. An interesting fact about the GBC is that the maximum of simultaneos colors that could happen in the screen is 56. So what if we do a color quantization with $56$ colors? I used $100$ neighbours, $10$ palette changes and $180$ iterations.

### Image obtained by GRASP in iteration 1:

![pokemon_yellow_1](https://github.com/user-attachments/assets/2422d831-8aa0-4dd6-977b-b702791b10e5)

### Image obtained by GRASP in iteration 2:

![pokemon_yellow_2](https://github.com/user-attachments/assets/0b1a3918-6a82-477d-b6b1-10faaf109225)

### Image obtained by GRASP in iteration 11:

![pokemon_yellow_11](https://github.com/user-attachments/assets/ea027276-f08f-4bb7-ac2a-c96e9226ecf9)

### Image obtained by GRASP in iteration 20:

![pokemon_yellow_20](https://github.com/user-attachments/assets/20336f7a-c053-4449-ac1f-176b8c7d4c6a)

### Image obtained by GRASP in iteration 90:

![pokemon_yellow_90](https://github.com/user-attachments/assets/50de2700-a8fd-42b3-b04a-009663a74d91)

## Cats image

![gatos](https://github.com/user-attachments/assets/794fc0bb-be4e-4ea6-bcce-34f500b674d8)

Now, what if we a real image? Let's see what happens. I used $64$ colors, $16$ palette changes, $100$ neighbours and $60$ iterations.

### Image obtained by GRASP in iteration 1:

![gatos_1](https://github.com/user-attachments/assets/37846fcf-26cf-4c1b-8f57-54b1e9f54826)

### Image obtained by GRASP in iteration 18:

![gatos_18](https://github.com/user-attachments/assets/caccff1d-c113-423c-9768-1af43520c165)

### Image obtained by GRASP in iteration 45:

![gatos_45](https://github.com/user-attachments/assets/4387deee-ff07-4d1a-883e-1c4e6b169394)

As you can see, fortunately in the first iteration GRASP got a good greedy solution and the image hasn't changed too much after that.
