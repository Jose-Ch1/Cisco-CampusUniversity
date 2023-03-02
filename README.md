### University campus 
![Universitary-Campus Cisco Proyects](https://user-images.githubusercontent.com/123495174/222539158-ed716b70-50cd-4190-948a-f3754b1e2ca8.png)



This is an university with 4 faculties with two campuses, the fist one is the biggest campus and have 3 building. The second one is  small and only have  the building of Science/Health
The faculties are: The faculties of Engineering/Computing, Art/Design, Science/health and Business. All students have labs with PC’s.
University campus
This is a university with 4 faculties with two campuses, the first one is the biggest campus and it has 3 buildings. The second is small and the building only has a faculty
The faculties are: The Faculties of Engineering/Computer Science, Art/Design, Science/Health and Business. All students have PC labs.

the buildings are distributed in:


- **Building 1:** in this building are the administrative department, HR and the finance department. Also have the faculty of business.
- **Building 2:** has the faculty of engineering and computing and also the faculty of art and design.
- **Building 3:**  in this building is the student laboratory and the IT department. The IT department is the web server and other servers of the university.
- **smaller campus:** on this campus is the faculty of sciences and health and also a student laboratory.

Details about the topology of the university
1. 	Each department/faculty have its own ip network.
2. 	Each department/faculty have its own vlan.
3. 	We will use RIPv2 to provide routing 
4. 	Each devise can acquire dynamic IP addresses from a DHCP server in the router.
5. We will use router-on-stick to separate Vlan

some configuration that we use during the proyet:
**To configure Vlans on a switch**
```html


hostname Admin
int range fa0/1-24
switchport mode access
switchport access vlan 11
do wr
exit


```
**Ip configuration in the principal routes**
```html

int se0/1/1
ip address 10.10.10.1 255.255.255.252
do wr 
exit

int se0/1/0
ip address 10.10.10.1 255.255.255.252
do wr 
exit

int gig0/0
ip address 20.0.0.1 255.255.255.252
do wr 
exit
```
**router-on-stick configuration on the main router and the small campus, also the DHCP pool configuration for each Vlan**
```html
  Router-on-stick
int gig0/0.11
encapsulation dot1q 11
ip address 192.168.1.1 255.255.255.0
do wr
exit 

DHCP config
 
service dhcp 
ip dhcp pool Admin-pool
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 192.168.1.1
do wr 
exit

```
**Configuration of RIPV2 to provide routing **
```html
-------------small-router 
router rip
version 2
network 192.168.9.0
network 192.168.100.0
network 10.10.10.0
exit
do wr

-------------Main-router 
router rip
version 2
network 10.10.10.0
network 10.10.10.4
network 192.168.1.0
network 192.168.2.0
network 192.168.3.0
network 192.168.4.0
network 192.168.5.0
network 192.168.6.0
network 192.168.7.0
network 192.168.8.0
exit
do wr
-----------cloud 
router rip
version 2
network 10.10.10.4
network 20.0.0.0
exit
do wr
```
