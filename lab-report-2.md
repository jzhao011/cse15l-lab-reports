# Lab Report 2

## Part 1

**Chat Server Code**
```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String s = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/add-message")) {
            String[] args = url.getQuery().split("&");
            String user = args[0].split("=")[1];
            String message = args[1].split("=")[1];
            
            s += String.format("%s: %s\n", message, user);
        }
        return s;
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

**Using Add Message**

![Image](/images/add-message-1.png)

Searching the url calls the ```handleRequest``` function in the code.

The relevant argument to this function is ```url```, a ```URI``` object that represents the internet address I had searched: ```http://localhost: 4000/add-message?s=How%20are%20you&user=john```.
The class also has a relevant value, ```String s```. Before the method was called, this had a value of ```"jesse: Hi\njohn: Hey\n"```.

After running the method, the value ```String s``` changed. It got updated to ```"jesse: Hi\njohn: Hey\njohn: How are you\n"```.

![Image](/images/add-message-2.png)

Like the previous example, searching this url also called the ```handleRequest``` function in the code.

The relevant argument to the function is ```URI url```, which represents the internet address ```http://localhost:4000/add-message?s=I%27m%20good&user=jesse```.
The class's relevant value, ```String s```, is now updated to ```"jesse: Hi\njohn: Hey\njohn: How are you\n"``` from the previous call to the method.

After running the method, ```String s``` got updated to ```"jesse: Hi\njohn: Hey\njohn: How are you\njesse: I'm good\n"```.

## Part 2

**Path to Private Key**

![Image](/images/keyabsolutepath.png)

The path is ```/Users/jesse/.ssh/id_rsa```

**Path to Public Key**

![Image](/images/publickeyabsolutepath.png)

The path is ```/home/linux/ieng6/oce/3g/jez011/.ssh/authorized_keys```

**Passwordless SSH connection**

![Image](/images/passwordlesslogin.png)

## Part 3
One thing I didn't know was how a URL is processed. I learned that the path is the part after the domain that looks like the path of a directory, such as ```/index.html```.
I also learned what a query was and that it was essentially how web servers take in arguments, which was very interesting. For example, the query in the Chat Server we made is ```s=I%27m%20good&user=jesse```, which passes in two arguments. The query is separated from the path by a question mark. This helped me better understand why URLs are structured the way they are.
