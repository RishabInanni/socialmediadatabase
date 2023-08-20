# Users
CREATE TABLE users(
id serial unique primary key,
username varchar(255) not NULL,
created_at timestamp default now()
);
select * from users;

# photos
create table photos(
	id serial primary key,
	image_url varchar (355) not null,
	user_id int not null,
	created_dat timestamp default now(),
	foreign key (user_id)references
users(id));
select * from photos;

# comments
create table comments(
	id serial primary key,
	comment_text varchar (355) not null,
	user_id int not null,
	photo_id int not null,
	created_dat timestamp default now(),
	foreign key (user_id)references
users(id),
foreign key (photo_id)references
photos(id));

select * from comments;

# likes

create table likes(
 user_id int not null,
 photo_id int not null,
 created_at timestamp default now(),
 foreign key(user_id) references users(id),
 foreign key(photo_id) references photos(id),
 primary key(user_id,photo_id)
);

select* from likes;

# follows

create table follows(
 follower_id int not null,
 followee_id int not null,
 created_at timestamp default now(),
 foreign key(follower_id) references users(id),
 foreign key(followee_id) references photos(id),
 primary key(follower_id,followee_id)
);
select * from follows;

# tags

create table tags(
id serial primary key,
tag_name varchar(255) unique not null,
created_at timestamp default now()
);
select * from tags;

# photo_tags
create table photo_tags(
 photo_id int not null,
 tag_id int not null,
 foreign key(photo_id) references photos(id),
 foreign key(tag_id) references tags(id),
 primary key(photo_id,tag_id)
);
select * from photo_tags;




# Inserting  values :


insert into users (username,created_at) values('rishab innani','2023-02-16 18:22:30'),
('pratham sharma,','2023-02-17 17:22:30');
insert into users (username,created_at) values('devi prasad mishra','2022-01-15 16:22:30'),
('binayak koirala,','2023-02-17 16:22:30');
select * from users;

insert into photos(image_url,user_id) values ('http://1.jpg',1),('http://2.jpg',2),('http://3.jpg',3),('http://4.jpg',4),('http://5.jpg',1);
insert into follows(follower_id,followee_id) values (2,1),(2,3),(2,4),(1,2),(3,1);
insert into comments(comment_text ,user_id,photo_id) values('jhakkas',2,1),('lovely',1,2);
insert into likes(user_id,photo_id) values (2,1),(2,4),(1,2);
insert into tags(tag_name) values('sunset'),('photography'),('winter');
insert into photo_tags(photo_id,tag_id) values (1,2),(5,1),(2,3);
insert into photo_tags(photo_id,tag_id) values (2,2),(3,2);
