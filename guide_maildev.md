Installer MailDev au global si nécessaire :
```shell
sudo npm install -g maildev
```

Puis lancer le serveur MailDev :
```shell
maildev --hide-extensions STARTTLS
```
Cette commande donne le lien d'accès à l'interface MailDev [[x]](http://0.0.0.0:1080/#/) et le port du serveur à renseigner dans le fichier ***.env***

Dans ***.env*** décommenter la ligne :
```php
MAILER_DSN=smtp://localhost
```

Puis ajouter le port *:1025*
```php
MAILER_DSN=smtp://localhost:1025
```

Pour utiliser des images, définir le chemin dans ***config/packages/twig.yaml*** en ajoutant
```yaml
    path:
        '%kernel.project_dir%/assets/images': images
```
ici il faut avoir un dossier ***assets/images*** à la racine du projet

### Envoyer un mail

```php
    /**
     * @Route("/mail", name="sendmail")
     */
    public function sendMail(MailerInterface $mailer)
    {
        $mail = (new Email())
            ->to('lisa.michallon.simplon@gmail.com')
            ->from('steve.jobs@apple.com')
            ->subject('IPHONE 13 GRATUIT !!!')

            // contenu du mail
            ->text('Un cadeau pour toi') // si texte uniquement
            ->html('<a href="#">Clique ici !</a>') // si html
            // les balises <img> ne fonctionnent pas dans html()
            // pour linker des images on utilise :
            ->embedFromPath('images/simplon.jpg'); // chemin part du dossier public
            
            // pièces jointes
           ->attachFromPath(''); // chemin part du dossier public
           
        $mailer->send($mail);

        return new Response('Email envoyé !');
    }
```

les USE nécessaires :
```php
use Symfony\Component\Mailer\MailerInterface;
use Symfony\Component\Mime\Email;
use Symfony\Component\HttpFoundation\Response;
```

### Envoyer un mail depuis un template Twig

```php
    /**
     * @Route("/mail-twig", name="sendmailtwig")
     */
    public function sendTwigMail(MailerInterface $mailer)
    {
        $mail = (new TemplatedEmail())
            ->to('lisa.michallon.simplon@gmail.com')
            ->from('steve.jobs@apple.com')
            ->subject('IPHONE 13 GRATUIT !!!')

            // contenu du mail 
            ->htmlTemplate('emails/test.html.twig'); // part du dossier templates

        $mailer->send($mail);

        return new Response('Email envoyé !');
    }
```

les USE nécessaires :
```php
use Symfony\Component\Mailer\MailerInterface;
use Symfony\Bridge\Twig\Mime\TemplatedEmail;
use Symfony\Component\HttpFoundation\Response;
```