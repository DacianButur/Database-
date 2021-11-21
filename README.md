Prima data creez baza de date. --> create database if not exists FarmaceuticalWareHouse; use farmaceuticalWareHouse;

Creez tabelul Product si setem ca si primary key idProduct --> create table Product (
idProduct int primary key not null,
productName varchar(25),
idCategory int,
price int
);


Dupa accea creez tabelul Category --> create table Category (
idCategory int primary key auto_increment not null,
productCategory varchar(25)
);


Facem alter table la Product ca sa adaugam fk ca sa facem legatura intre tabele --> alter table product add foreign key(idCategory) references category(idCategory) on delete set null;

Creez tabelul client --> create table Client(
idClient int primary key auto_increment not null,
clientName varchar(25)
);

Creez tabelul orders --> create table Orders (
idCategory int,
idProduct int,
idClient int,
Date date,
productUnits int,
foreign key(idProduct) references product(idProduct) on delete set null,
foreign key(idCategory) references category(idCategory) on delete set null,
foreign key(idClient) references client(IdClient) on delete set null
);

Dupa care populam tabelele prin comenzile de insert --> 
-- insert for category table
insert into category values (1,'Anabolizante');
insert into category values(2,'Antibiotice');
insert into category values(3,'Vitamine');
insert into category values(4,'Analgezice');

-- insert for products table
insert into product values(5,'Ketonal',4,15);
insert into product values(6,'Trenbolone',1,100);
insert into product values(7,'Anavar',1,500);
insert into product values(8,'Dihidroxi-Steron',1,90);
insert into product values(9,'HGH',1,1000);
insert into product values(10,'Vit.K',3,89);
insert into product values(11,'Vit.C',3,34);
insert into product values(12,'Vit,D3',3,76);
insert into product values(13,'Indometacin',2,48);
insert into product values(14,'Ketoprofen',4,46);
insert into product values(15,'Piroxicam',4,15);
insert into product values(16,'Augumentin',2,80);
insert into product values(17,'Clavamox',2,150);
insert into product values(18,'Oxacilina',2,100);

-- insert for client table
insert into client values(200,'Farmacia_Dona');
insert into client values(300,'Farmacia_Vlad');
insert into client values(400,'Farmacia_Dacia');
insert into client values(500,'Farmacia_Help_Net');


-- insert for order table
insert into orders values(1,8,200,'2021-01-14',15);
insert into orders values(2,13,300,'2021-02-02',25);
insert into orders values(4,14,300,'2021-01-15',150);
insert into orders values(3,10,500,'2021-01-18',250);
insert into orders values(4,15,400,'2021-02-19',300);
insert into orders values(2,16,200,'2021-08-22',350);
insert into orders values(2,17,300,'2021-02-12',150);
insert into orders values(2,18,200,'2021-02-11',250);
insert into orders values(3,12,300,'2021-04-29',150);
insert into orders values(4,14,300,'2021-03-12',10);
insert into orders values(2,15,200,'2021-12-30',15);
insert into orders values(3,12,200,'2021-02-21',400);
insert into orders values(3,12,400,'2021-05-14',500);
insert into orders values(4,15,200,'2021-02-12',15);
insert into orders values(1,7,300,'2021-08-26',25);
insert into orders values(1,6,200,'2021-08-01',49);
insert into orders values(1,9,200,'2021-03-14',77);
insert into orders values(3,11,500,'2021-07-12',66);
insert into orders values(2,11,200,'2021-12-09',33);
insert into orders values(3,12,200,'2021-08-12',334);
insert into orders values(4,13,200,'2021-07-25',112);
insert into orders values(1,9,200,'2021-02-15',10);
insert into orders values(1,9,300,'2021-03-13',24);
insert into orders values(2,18,300,'2021-09-24',15);
 
 Dupa aceea vom face selecturile pentru a afla informatiile necesare -->
 -- select pentru cerinta nr.1
-- select ca sa aflam nr-ul de comenzi din luna august a farmaciei dona si valoare lor medie + valoarea absoluta
select orders.idClient,orders.date, orders.idProduct,product.price,orders.productUnits,client.clientName, orders.productUnits * product.price as totalPrice from orders join product on orders.idProduct = product.idProduct join client on orders.idClient = client.idClient where orders.idClient = 200 and month(date) = 08;

-- select ca sa aflam valoarea medie pe comanda ale celor 3 comenzi.
select orders.idClient,client.clientName, avg(orders.productUnits * product.price) as totalPrice from orders join product on orders.idProduct = product.idProduct join client on orders.idClient = client.idClient where orders.idClient = 200 and month(date) = 08;

-- select ca sa vedem suma totala a comenzilor
select orders.idClient, client.clientName, sum(orders.productUnits * product.price) as totalPrice from orders join product on orders.idProduct = product.idProduct join client on orders.idClient = client.idClient where orders.idClient = 200 and month(date) = 08;


-- select pentru cerinta nr. 2:
select orders.idClient, client.clientName, orders.idCategory,orders.date, orders.productUnits * product.price as totalPrice from orders join client on orders.idClient = client.idClient join product on orders.idProduct = product.idProduct where orders.idCategory = 2 and year(date) = 2021 and client.clientName = 'Farmacia_Vlad';
-- vedem ca farmacia Vlad a avut 3 comenzi de antibiotice in 2021 cu o medie de 8400RON pe comanda.

-- select pentru cerinta nr.3:
select orders.idClient, client.ClientName, sum(orders.productUnits * product.price) as totalPrice from orders join client on orders.idClient = client.idClient join product on orders.idProduct = product.IdProduct group by clientName order by totalPrice desc;
-- vedem ca farmacia care a cheltuit cei mai multi bani pe comenzi in 2021 a fost Farmacia Dona.
