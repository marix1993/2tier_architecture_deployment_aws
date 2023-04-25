**What is a VPC?**
-  A virtual private cloud (VPC) is a secure, 
isolated private cloud hosted within a public cloud. 
VPC customers can run code, store data, host websites, 
and do anything else they could do in an ordinary private 
cloud, but the private cloud is hosted remotely by a public cloud provider.

**Why? What are the benefits of VPC?**

- Agility
- Security
- Hybrid clouds are easy to deploy
- Improved performance
- Availability
- Satisfied customers
- Increased resources to channel innovation

**What is internet gateway?**

- An internet gateway is a horizontally scaled, 
redundant, and highly available VPC component that 
allows communication between your VPC and the internet. 
It supports IPv4 and IPv6 traffic. It does not cause availability 
risks or bandwidth constraints on your network traffic.


**What is a subnet?**

- A subnet, or subnetwork, is a network inside a network. 
Subnets make networks more efficient. Through subnetting, 
traffic can travel a shorter distance without passing 
through unnecessary routers to reach its destination.

**What is CIDR block?**

- A CIDR block is a collection of IP addresses 
that share the same network prefix and number of bits. 
A large block consists of more IP addresses and a small suffix. 
The Internet Assigned Numbers Authority (IANA) assigns large CIDR 
to regional internet registries (RIR).



**What are route tables?**

- A route table contains a set of rules, called routes, 
that determine where network traffic from your subnet or gateway is directed.

![diag.png](files%2Fdiag.png)

Step 1. Create `VPC`:
-

1. On AWS console find `VPC` in the search bar and
then in `Services` click on `VPC`.
2. Click `Create VPC`.
3. Complete VPC settings :
- VPC only
- name: `mateusz-tech221-vpc`
- `IPv4 CIDR` set to `10.0.0.0/16` 


Step 2. Create `Internet Gateway`:
-

1. On the left panel find `Internet gateway`.
2. Click `Create Internet Gateway`.
3. Type `mateusz-tech221-IG-vpc`.
4. Click `Create internet gateway`.
5. On next page click on `Actions` and `Attach to VPC`.
6. Search for previous `VPC` and click `Attach internet gateway`.

Step 3. Create `Subnet` (public/private).
- 
**For public subnet:**
1. On the left panel find `Subnets`.
2. Click `Create subnets`,
3. Find our previous `mateusz VPC ID`.
4. Subnet name: `mateusz-tech221-public-app-subnet`.
5. `IPv4 CIDR` set to `10.0.5.0/16` (make sure to add your specified number - in this case 10.0.`5`.0/16 !)
6. Click `Create subnet`.

**For private subnet:**
1. On the left panel find `Subnets`.
2. Click `Create subnets`,
3. Find our previous `mateusz VPC ID`.
4. Subnet name: `mateusz-tech221-private-db-subnet`.
5. `IPv4 CIDR` set to `10.0.51.0/16` (make sure to add your specified number - in this case 10.0.`51`./16 !)
6. Click `Create subnet`.

Step 4 . Create a `Route Table`:
-

1. On the left panel find `Route tables`.
2. Click `Create route table`.
3. Type name : `mateusz-tech221-public-RT`.
4. Select proper `mateusz-VPC`.
5. Click `Create route table`.
6. Now we need to connect our `route table` to `public subnet`
- find `Subnet association`
- click `Edit subnet association`
- select your public subnet and `Save`.

Step 5. Attach `Route table` to `Internet Gateway`.
-

1. In `Route table` find `Routes`.
2. Click `Edit routes`.
3. One route is already there `10.0.0.0/16` but we need to add another one.
Click `Add route`.
4. Set `Destination` to `0.0.0.0` & `Target` to `Internet Gateway` - choose ours.
5. Click save changes.

Step 6. Create AMIs for our `app` and `db` instances.
-
1. Go to our running instances.
2. Select `App` instance.
3. Go to `Actions` > `Image and templates` > `Create Image`.
4. Name it with our convention `mateusz_tech221_app_AMI`.
5. Click `Create image`.
6. For `DB AMI` follow the same steps but change name to `mateusz_tech221_db_AMI`.

Step 7. Launch instances from AMI.
-

1. In `Images` click `AMIs`.
2. Find your `APP AMI` and click `Launch instance from AMI`.
3. Now we need to edit our `network settings` in two ways:

- **public for App instance:**
   
    a) Select your VPC

    b) Select your public Subnet

    c) Make sure to `enable` Auto-assign Public IP

    d) Add SSH connection (port 22 / MY IP)

    e) Add HTTP connection (port 80 /Anywhere)

    f) Add Custom TCP (port 3000 /Anywhere)
2. Find your `DB AMI` and click `Launch instance from AMI`.
3. Now we need to edit our `network settings` in two ways:

- **private for DB instance:**

    a) Select your VPC

    b) Select your private subnet

    c) Make sure to `disable` Auto-assign Public IP

    d) Add Custom TCP (port 27017 /Anywhere)

Step 8. Connect App and DB instances:
-

1. In GitBash navigate to `.ssh` folder.
2. Copy your `App` ssh link changing `root` to `ubuntu` and SSH into your App in GitBash.
3. Check if your env variable DB_HOST is persistent. If it is change it:
- type: `sudo nano .bashrc`
- find `DB_HOST=` and change ip to your `private ip` from `DB instance`.
- `ctrl + s` to save, `ctrl + x` to exit
4. Use `source .bashrc` to restart the script.
5. Use `cd app` to navigate to App folder
6. `npm install` to install the app.
7. If it's not seeded with database use : `node seeds/seed.js`.
8. Next use: `node app.js` or `npm start`.
9. Now in browser paste your `<App instance ip>`:3000/posts.














