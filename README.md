### **Workshop Algorithmique**
*Aurélien Michaud & Guilhem Rebéjac*

#### Exercice 1 : Ne garder que le vert
```cpp
void vert()
{
    sil::Image image{"images/logo.png"};
    // TODO: modifier l'image
    // On passe sur tous les x et tous les y, et on accède au pixel correspondant :
    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            image.pixel(x,y).r = 0.f;
            image.pixel(x,y).b = 0.f;
        }
    }

    image.save("output/vert.png");
}
```

![image](/output/vert.png)

#### Exercice 2 : Echanger les canaux
```cpp
void canaux()
{
    sil::Image image{"images/logo.png"};
    // TODO: modifier l'image
    // Ou alors, si on n'a pas besoin de connaître le x et le y, on a une syntaxe plus concise pour itérer sur tous les pixels :
    for (glm::vec3& color : image.pixels())
    {
        std::swap(color.r, color.b);
    }

    image.save("output/canaux.png");    
}
```
![image](/output/canaux.png)

#### Exercice 3 : Noir & Blanc
*Version moyenne des couleurs :*
```cpp
void noirblanc_moyenne()
{
    sil::Image image{"images/logo.png"};
    for (glm::vec3& color : image.pixels())
    {
        float moyenne{};
        moyenne = (color.r+color.g+color.b)/3;
        color.r = moyenne;
        color.g = moyenne;
        color.b = moyenne;
    }

    image.save("output/noirblanc_moyenne.png");  
}
```
![image](/output/noirblanc_moyenne.png)

*Version filtre couleur (ici rouge) :*
```cpp
void noirblanc_filtrerouge()
{
    sil::Image image{"images/logo.png"};
    for (glm::vec3& color : image.pixels())
    {
        float moyenne{};
        moyenne = (color.g+color.b)/2;
        color.r = moyenne;
        color.g = moyenne;
        color.b = moyenne;
    }

    image.save("output/pouet.png");
}
```
![image](/output/noirblanc_filtrerouge.png)

*Version couleur dominante :*
```cpp
void noirblanc_couleurdominante()
{
    sil::Image image{"images/logo.png"};
    for (glm::vec3& color : image.pixels())
    {
        if((color.r>=color.g)&&(color.r>=color.b))
        // ici je ne peux pas utiliser (color.r>(color.g||color.b))
        {
            float moyenne{};
            moyenne = (color.g+color.b)/2;
            color.r = moyenne;
            color.g = moyenne;
            color.b = moyenne;
        }
        if((color.g>=color.b)&&(color.g>=color.r))
        {
            float moyenne{};
            moyenne = (color.b+color.r)/2;
            color.r = moyenne;
            color.g = moyenne;
            color.b = moyenne;
        }
        if((color.b>=color.r)&&(color.b>=color.g))
        {
            float moyenne{};
            moyenne = (color.r+color.g)/2;
            color.r = moyenne;
            color.g = moyenne;
            color.b = moyenne;
        }
    }
    // ça fait un truc stylé si on mets > au lieu de >= à cause des floats
    image.save("output/noirblanc_couleurdominante.png");    
}
```
![image](/output/noirblanc_couleurdominante.png)

#### Exercice 4 : Négatif
```cpp
void negatif()
{
    sil::Image image{"images/logo.png"};
    for (glm::vec3& color : image.pixels())
    {
        color.r = 1 - color.r;
        color.g = 1 - color.g;
        color.b = 1 - color.b;
    }

    image.save("output/negatif.png");  
}
```
![image](/output/negatif.png)

#### Exercice 5 : Dégradé
```cpp
void degrade()
{
    sil::Image image{300, 200};
    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            image.pixel(x,y).r = x/(image.width()*1.f);
            image.pixel(x,y).g = x/(image.width()*1.f);
            image.pixel(x,y).b = x/(image.width()*1.f);
        }
    }

    image.save("output/degrade.png");   
}
```
![image](/output/degrade.png)

#### Exercice 6 : Miroir
```cpp
void miroir()
{
    sil::Image image{"images/logo.png"};
    for (int x{0}; x < image.width()/2; x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            // image.pixel(x, y) = image.pixel(image.width()-1-x, y); cette ligne seul incrémente la partie droite sur la gauche seulement
            std::swap(image.pixel(x,y), image.pixel(image.width()-1-x,y));
        }
    }

    image.save("output/miroir.png"); 
}
```
![image](/output/miroir.png)
**Difficulté :** déterminer le "-1-x" qui permet de trouver le bon pixel à échanger.

#### Exercice 7 : Image bruitée
```cpp
void image_bruitee()
{
    sil::Image image{"images/logo.png"};
    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            int boolean = random_int(0,2);
            if(boolean==1)
            {
                image.pixel(x,y).r = random_float(0,1);
                image.pixel(x,y).g = random_float(0,1);
                image.pixel(x,y).b = random_float(0,1);
            }
        }
    }

    image.save("output/image_bruitee.png"); 
}
```
![image](/output/image_bruitee.png)

#### Exercice 8 : Rotation de 90°
```cpp
void rotation_90()
{
    sil::Image image{"images/logo.png"};
    sil::Image newimage{image.height(), image.width()};
    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            newimage.pixel(newimage.width()-1-y, x) = image.pixel(x,y);
        }
    }

    newimage.save("output/rotation_90.png"); 
}
```
![image](/output/rotation_90.png)

#### Exercice 9 : RGB split
```cpp
void rgb_split()
{
    sil::Image image{"images/logo.png"};
    sil::Image newimage{image.width(),image.height()};

    int number{};
    std::cout << "Entrez un nombre entre 0 et " << image.width()/2 << " : ";
    std::cin >> number;
    
    for (int x{0}; x < number; x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            newimage.pixel(x,y).r = image.pixel(x+number,y).r;
            newimage.pixel(x,y).g = image.pixel(x,y).g;
            newimage.pixel(x,y).b = image.pixel(x,y).b;
        }
    }
    
    for (int x{number}; x < (image.width()-1-number); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            newimage.pixel(x,y).r = image.pixel(x+number,y).r;
            newimage.pixel(x,y).g = image.pixel(x,y).g;
            newimage.pixel(x,y).b = image.pixel(x-number,y).b;
        }
    }

    for (int x{(image.width()-1-number)}; x < (image.width()-1); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            newimage.pixel(x,y).r = image.pixel(x,y).r;
            newimage.pixel(x,y).g = image.pixel(x,y).g;
            newimage.pixel(x,y).b = image.pixel(x-number,y).b;
        }
    }

    newimage.save("output/rgb_split.png"); 
}
```
![image](/output/rgb_split.png)
**Difficulté :** réussir à bien coder pour que les conditions aux bords ne bug pas.

#### Exercice 10 : Luminosité
```cpp
void luminosite()
{
    float puissance{};
    std::cout << "Entrez une valeur de luminosite entre 0 et 10 : ";
    std::cin >> puissance;
    sil::Image image{"images/photo_faible_contraste.jpg"};
    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            image.pixel(x,y) = glm::vec3{std::pow(image.pixel(x,y).r, puissance),std::pow(image.pixel(x,y).g, puissance),std::pow(image.pixel(x,y).b, puissance)};
        }
    }

    image.save("output/luminosite.png"); 
}
```
![image](/output/luminosite.png)
**Difficulté :** comprendre l'utilité des fonctions polynômes dans la gestion de la luminosité.

#### Exercice 11 : Disque
```cpp
void disque()
{
    sil::Image image{500, 500};

    int rayon{};
    std::cout << "Entrez la taille du rayon du disque entre 0 et " << image.width()/2 << " pixels : ";
    std::cin >> rayon;

    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            float const distance {glm::distance(
                glm::vec2(x,y),
                glm::vec2(image.width()/2, image.height()/2)
            )};
            if (distance<=rayon)
            {
                image.pixel(x,y) = glm::vec3{1.f};
            }
        }
    }

    image.save("output/disque.png"); 

}
```
![image](/output/disque.png)
**Difficulté :** trouver la fonction permettant de déterminer si le pixel appartient ou non au cercle.

#### Exercice 12 : Cercle
```cpp
void cercle()
{
    sil::Image image{500, 500};

    int rayon{};
    std::cout << "Entrez la taille du rayon du cercle entre 0 et " << image.width()/2 << " pixels : ";
    std::cin >> rayon;    
    
    int epaisseur{};
    std::cout << "Entrez l'epaisseur du cercle : ";
    std::cin >> epaisseur;

    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            float const distance {glm::distance(
                glm::vec2(x,y),
                glm::vec2(image.width()/2, image.height()/2)
            )};
            if (distance<=rayon && distance>=(rayon-(epaisseur/2)))
            {
                image.pixel(x,y) = glm::vec3{1.f};
            }
        }
    }

    image.save("output/cercle.png"); 

}
```
![image](/output/cercle.png)

#### Exercice 13 : Rosace
```cpp
#define M_PI 3.14

void rosace()
{
    sil::Image image{500, 500};

    int nombre{};
    std::cout << "Entrez le nombre de cercle dans la rosace : ";
    std::cin >> nombre;

    int rayon{};
    std::cout << "Entrez la taille du rayon du cercle entre 0 et " << image.width()/4 << " pixels : ";
    std::cin >> rayon;    
    
    int epaisseur{};
    std::cout << "Entrez l'epaisseur du cercle : ";
    std::cin >> epaisseur;

    for (int x{0}; x < image.width(); x++)
    {
        for (int y{0}; y < image.height(); y++)
        {
            float const distance {glm::distance(
                glm::vec2(x,y),
                glm::vec2(image.width()/2, image.height()/2)
            )};
            if (distance<=rayon && distance>=(rayon-(epaisseur/2)))
            {
                image.pixel(x,y) = glm::vec3{1.f};
            }
        }
    }

    for (float teta{0}; teta<(2*3.14); teta+=(2*3.14)/nombre)
    {
        std::cout << teta << std::endl;
        for (int x{0}; x < image.width(); x++)
        {
            for (int y{0}; y < image.height(); y++)
            {
                float const distance {glm::distance(
                    glm::vec2(x,y),
                    glm::vec2((image.width()/2)+((rayon-(epaisseur/2))*std::cos(teta)), (image.height()/2)+((rayon-(epaisseur/2))*std::sin(teta)))
                )};
                if (distance<=rayon && distance>=(rayon-(epaisseur/2)))
                {
                    image.pixel(x,y) = glm::vec3{1.f};
                }
            }
        } 
    }

    image.save("output/rosace.png"); 

    std::string reponse{""};
    std::cout << "Voulez-vous effacer la partie exterieure au cercle central ?" << std::endl;
    std::cout << "Repondez par oui ou non : ";
    std::cin >> reponse;

    if (reponse == "oui")
    {
        for (int x{0}; x < image.width(); x++)
        {
            for (int y{0}; y < image.height(); y++)
            {
                float const distance {glm::distance(
                    glm::vec2(x,y),
                    glm::vec2(image.width()/2, image.height()/2)
                )};
                if (distance>rayon)
                {
                    image.pixel(x,y) = glm::vec3{0};
                }
            }
        }

        std::string degrade{""};
        std::cout << "Voulez-vous faire un degrade dans la rosace ?" << std::endl;
        std::cout << "Repondez par oui ou non : ";
        std::cin >> degrade;

        if (degrade == "oui")
        {
            std::string choix{""};
            std::cout << "Voulez-vous faire un degrade sur la rosace, dans la rosace ou inversee ?" << std::endl;
            std::cout << "Repondez par sur, dans ou inv : ";
            std::cin >> choix;

            if (choix == "sur")
            {
                for (int x{0}; x < image.width(); x++)
                {
                    for (int y{0}; y < image.height(); y++)
                    {
                        float const distance {glm::distance(
                            glm::vec2(x,y),
                            glm::vec2(image.width()/2, image.height()/2)
                        )};
                        if (distance<=rayon && image.pixel(x,y) == glm::vec3{1.f})
                        {
                            image.pixel(x,y).r = x/(image.width()*1.f);
                            image.pixel(x,y).g = x/(image.width()*1.f);
                            image.pixel(x,y).b = x/(image.width()*1.f);
                        }
                    }
                }
            }
            else if (choix == "dans")
            {
                for (int x{0}; x < image.width(); x++)
                {
                    for (int y{0}; y < image.height(); y++)
                    {
                        float const distance {glm::distance(
                            glm::vec2(x,y),
                            glm::vec2(image.width()/2, image.height()/2)
                        )};
                        if (distance<=rayon && image.pixel(x,y) != glm::vec3{1.f})
                        {
                            image.pixel(x,y).r = x/(image.width()*1.f);
                            image.pixel(x,y).g = x/(image.width()*1.f);
                            image.pixel(x,y).b = x/(image.width()*1.f);
                        }
                    }
                }                
            }
            else if (choix == "inv")
            {
                for (int x{0}; x < image.width(); x++)
                {
                    for (int y{0}; y < image.height(); y++)
                    {
                        float const distance {glm::distance(
                            glm::vec2(x,y),
                            glm::vec2(image.width()/2, image.height()/2)
                        )};
                        if (distance<=rayon && image.pixel(x,y) != glm::vec3{1.f})
                        {
                            image.pixel(x,y).r = x/(image.width()*1.f);
                            image.pixel(x,y).g = x/(image.width()*1.f);
                            image.pixel(x,y).b = x/(image.width()*1.f);
                        }
                        if (distance<=rayon && image.pixel(x,y) == glm::vec3{1.f})
                        {
                            image.pixel(x,y).r = 1-(x/(image.width()*1.f));
                            image.pixel(x,y).g = 1-(x/(image.width()*1.f));
                            image.pixel(x,y).b = 1-(x/(image.width()*1.f));
                        }                        
                    }
                }                
            }
        }   
    }

    if (reponse == "non")
    {
        std::string degrade{""};
        std::cout << "Voulez-vous faire un degrade dans la rosace ?" << std::endl;
        std::cout << "Repondez par oui ou non : ";
        std::cin >> degrade;

        if (degrade == "oui")
        {
            std::string choix{""};
            std::cout << "Voulez-vous faire un degrade sur la rosace, dans la rosace ou inversee ?" << std::endl;
            std::cout << "Repondez par sur, dans ou inv : ";
            std::cin >> choix;

            if (choix == "sur")
            {
                for (int x{0}; x < image.width(); x++)
                {
                    for (int y{0}; y < image.height(); y++)
                    {
                        if (image.pixel(x,y) == glm::vec3{1.f})
                        {
                            image.pixel(x,y).r = x/(image.width()*1.f);
                            image.pixel(x,y).g = x/(image.width()*1.f);
                            image.pixel(x,y).b = x/(image.width()*1.f);
                        }
                    }
                }
            }
            else if (choix == "dans")
            {
                for (float teta{0}; teta<(2*3.14); teta+=(2*3.14)/nombre)
                {

                    for (int x{0}; x < image.width(); x++)
                    {
                        for (int y{0}; y < image.height(); y++)
                        {
                            float const distance {glm::distance(
                                glm::vec2(x,y),
                                glm::vec2((image.width()/2)+((rayon-(epaisseur/2))*std::cos(teta)), (image.height()/2)+((rayon-(epaisseur/2))*std::sin(teta)))
                            )};
                            if (distance<=rayon && distance>=(rayon-(epaisseur/2)) && image.pixel(x,y) == glm::vec3{0.f})
                            {
                                image.pixel(x,y).r = x/(image.width()*1.f);
                                image.pixel(x,y).g = x/(image.width()*1.f);
                                image.pixel(x,y).b = x/(image.width()*1.f);
                            }
                        }
                    } 
                }                
            }
            else if (choix == "inv")
            {
                for (float teta{0}; teta<(2*3.14); teta+=(2*3.14)/nombre)
                {

                    for (int x{0}; x < image.width(); x++)
                    {
                        for (int y{0}; y < image.height(); y++)
                        {
                            float const distance {glm::distance(
                                glm::vec2(x,y),
                                glm::vec2((image.width()/2)+((rayon-(epaisseur/2))*std::cos(teta)), (image.height()/2)+((rayon-(epaisseur/2))*std::sin(teta)))
                            )};
                            if (distance<=rayon && distance>=(rayon-(epaisseur/2)) && image.pixel(x,y) != glm::vec3{1.f})
                            {
                                image.pixel(x,y).r = x/(image.width()*1.f);
                                image.pixel(x,y).g = x/(image.width()*1.f);
                                image.pixel(x,y).b = x/(image.width()*1.f);
                            }
                            if (distance<=rayon && distance>=(rayon-(epaisseur/2)) && image.pixel(x,y) == glm::vec3{1.f})
                            {
                                image.pixel(x,y).r = 1-(x/(image.width()*1.f));
                                image.pixel(x,y).g = 1-(x/(image.width()*1.f));
                                image.pixel(x,y).b = 1-(x/(image.width()*1.f));
                            }                        
                        }
                    }                
                }
            }           
        }
    }

    image.save("output/rosace_modif1.png");

    std::string couleur{""};
    std::cout << "Voulez-vous faire un degrade de couleur ?" << std::endl;
    std::cout << "Repondez par oui ou non : ";
    std::cin >> couleur;

    if (couleur == "oui")
    {
        sil::Image image_couleur{500, 500};
        for (int x{0}; x < image.width(); x++)
        {
            for (int y{0}; y < image.height(); y++)
            {
                glm::vec2 point(x,y);
                glm::vec2 centre(image.width()/2, image.height()/2);

                
               float angle = atan2(((point[0]-centre[0])),((point[1]-centre[1])));
                    glm::vec3 rgb = rgb_from_hsl((angle+M_PI)*180.f / M_PI, 0.7f, 0.7f);
                    image_couleur.pixel(x,y) = rgb*image.pixel(x,y) ;
            }    
        }   
             
        image_couleur.save("output/rosace_modif2.png");

    }
}
```
*Fonction de colorisation cercle chromatique :*
```cpp
glm::vec3 rgb_from_hsl(float h, float s, float l)
{
    float c = (1.f-(abs(2.f*l-1.f)))*s;
    float x = c*(1-abs(std::fmod((h/60.f),2.f)-1));
    float m = l-(c/2.f);

    glm::vec3 rgbprim(0,0,0);

    if (0 <= h && h < 60)
    {
        rgbprim.r = c;
        rgbprim.g = x;
    }
    else if (60 <= h && h < 120)
    {
        rgbprim.r = x;
        rgbprim.g = c;        
    }
    else if (120 <= h && h < 180)
    {
        rgbprim.g = c;
        rgbprim.b = x;        
    }
    else if (180 <= h && h < 240)
    {
        rgbprim.g = x;
        rgbprim.b = c;        
    }
    else if (240 <= h && h < 300)
    {
        rgbprim.r = x;
        rgbprim.b = c;        
    }
    else if (300 <= h && h <= 360)
    {
        rgbprim.r = c;
        rgbprim.b = x;        
    }

    glm::vec3 rgb(0,0,0);
    rgb.r = (rgbprim.r + m);
    rgb.g = (rgbprim.g + m);
    rgb.b = (rgbprim.b + m);

    return rgb;
}
```
![image](/output/rosace.png)
**Difficulté :** déterminer la fonction trigonométrique permettant de trouver les centres des cercles extérieurs sur le principal.

![image](/output/rosace_modif1.png)
*On remarque ci-dessus un bug de dégradé au niveau du cercle central et des intersections de cercles.* - non résolu -

![image](/output/rosace_modif2.png)

#### Exercice 14 : Mosaïque
```cpp
void mosaique()
{
    sil::Image image{"images/logo.png"};

    int taille{};
    std::cout << "Entrez le nombre de symbole de la mosaique tel que taille = nombre x nombre : ";
    std::cin >> taille;

    sil::Image newimage{image.width()*taille,image.height()*taille};

    for (int x{0}; x < newimage.width(); x++)
    {
        for (int y{0}; y < newimage.height(); y++)
        {
            newimage.pixel(x,y) = image.pixel(x%image.width(),y%image.height());
        }
    }

    newimage.save("output/mosaique.png"); 

}
```
![image](/output/mosaique.png)
**Difficulté :** comprendre l'utilité du modulo dans la simplification du code *(voir mosaïque miroir - version d'incrémentation par ligne/colonne)*.

#### Exercice 15 : Mosaïque miroir
*Version d'incrémentation par ligne/colonne :*
```cpp
void mosaique_miroir_complique()
{
    sil::Image image{"images/logo.png"};

    int taille{};
    std::cout << "Entrez le nombre de symbole de la mosaique tel que taille = nombre x nombre : ";
    std::cin >> taille;

    sil::Image newimage{image.width()*taille,image.height()*taille};

    for (int x{0}; x < newimage.width(); x++)
    {
        for (int y{0}; y < newimage.height(); y++)
        {
            newimage.pixel(x,y) = glm::vec3{1.f};
            if(x<image.width() && y<image.height())
            {
                newimage.pixel(x,y) = image.pixel(x,y);                
            }
        }
    }

    int x{};
    int y{0};
    for(int count1{0}; count1<taille; count1++)
    {
        while(y>=count1*image.height() && y<(count1+1)*image.height())
        {   
            x = 0;
            if(y/image.height()>=1)
            {
                while(x>=0 && x<image.width())
                {
                    newimage.pixel(x,y) = newimage.pixel(x,y-2*(y%image.height()));
                    x++;
                }
            }
            else
            {
                x = image.width();
            }

            for(int count2{1}; count2<taille; count2++)
            {
                while(x>=count2*image.width() && x<(count2+1)*image.width())
                {
                    newimage.pixel(x,y) = newimage.pixel(x-2*(x%image.width()),y);
                    x++;
                }
            }
            y++;
        }
    }

    newimage.save("output/mosaique_miroir_complique.png"); 

}
```
*On remarque ci-dessous un bug de décalage de 1 pixel entre les itérations.*
![image](/output/mosaique_miroir_complique.png)
**Difficulté :** le codage de l'incrémentation sur la ligne puis sur le premier pattern de la ligne du dessus en colonne est beaucoup plus compliqué que la version ci-après.
De plus, la version ligne/colonne à besoin du pattern précédent pour créer le suivant. Tandis que la version direct permet de caractériser n'importe quel pixel de l'image en fonction de l'image initiale. 

*Version de caractérisation d'un pixel quelconque directement par rapport au pattern initial :*
```cpp
void mosaique_miroir_simple()
{
    sil::Image image{"images/logo.png"};

    int taille{};
    std::cout << "Entrez le nombre de symbole de la mosaique tel que taille = nombre x nombre : ";
    std::cin >> taille;

    sil::Image newimage{image.width()*taille,image.height()*taille};
    for (int x{0}; x < newimage.width(); x++)
    {
        for (int y{0}; y < newimage.height(); y++)
        {
            if(x/image.width()%2==0 && y/image.height()%2==0)
            {
                newimage.pixel(x,y) = image.pixel(x%image.width(),y%image.height());
            }
            if(x/image.width()%2==0 && y/image.height()%2==1)
            {
                newimage.pixel(x,y) = image.pixel(x%image.width(),image.height()-1-(y%image.height()));
            }
            if(x/image.width()%2==1 && y/image.height()%2==0)
            {
                newimage.pixel(x,y) = image.pixel(image.width()-1-(x%image.width()),y%image.height());
            }
            if(x/image.width()%2==1 && y/image.height()%2==1)
            {
                newimage.pixel(x,y) = image.pixel(image.width()-1-(x%image.width()),image.height()-1-(y%image.height()));
            }
        }
    }

    newimage.save("output/mosaique_miroir_simple.png"); 

}
```
![image](/output/mosaique_miroir_simple.png)

```cpp

```
*© 2023 AMGR, Inc. Tous droits réservés*
