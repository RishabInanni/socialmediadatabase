# socialmediadatabase


CREATE TABLE users(
id serial unique primary key,
username varchar(255) not NULL,
created_at timestamp default now()
);
select * from users;


create table photos(
	id serial primary key,
	image_url varchar (355) not null,
	user_id int not null,
	created_dat timestamp default now(),
	foreign key (user_id)references
users(id));
select * from photos;


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

create table likes(
 user_id int not null,
 photo_id int not null,
 created_at timestamp default now(),
 foreign key(user_id) references users(id),
 foreign key(photo_id) references photos(id),
 primary key(user_id,photo_id)
);

select* from likes;

create table follows(
 follower_id int not null,
 followee_id int not null,
 created_at timestamp default now(),
 foreign key(follower_id) references users(id),
 foreign key(followee_id) references photos(id),
 primary key(follower_id,followee_id)
);
select * from follows;

create table tags(
id serial primary key,
tag_name varchar(255) unique not null,
created_at timestamp default now()
);
select * from tags;

create table photo_tags(
 photo_id int not null,
 tag_id int not null,
 foreign key(photo_id) references photos(id),
 foreign key(tag_id) references tags(id),
 primary key(photo_id,tag_id)
);
select * from photo_tags;




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

/* queries */
 
select * from users
order by created_at
limit 3;

/* days in which most user registed in app*/

select extract(dow from created_at) as "day of week",count(*)
from users
group by 1
order by 2 desc;

select to_char(created_at,'Dy') as day,
count(*) as total
from users
group by day
order by total desc;

insert into users (username,created_at) values('aryan','2023-02-16 18:22:30');

/* to find inactive user*/
select username from users
left join photos on users.id=photos.user_id
where photos.id is null;

/*how many times the aaverage user posts*/
select round((select count(*) from photos)/(select count(*)from users),2);

/* to find most active user*/


select users.username ,count(photos.image_url) from users
join photos on users.id=photos.user_id
group by users.id
order by 2 desc;


/*most used hastags*/

select tag_name ,count(tag_name) as total
from tags
join photo_tags on tags.id=photo_tags.tag_id
group by tags.id
order by total desc;

/* no of users who posted atleast once*/

select count(distinct(users.id)) as total_no_of_usr_with_post
from users
join photos on users.id=photos.user_id;

  
/*users who have not commented */
select username,comment_text
from users
left join comments on users.id=comments.user_id
group by users.id,comment_text
having comment_text is null;
 
 
/* no of people who never commented on any of the post*/
 select count(*) from
 ( select username,comment_text
  from users 
  left join comments on users.id=comments.user_id
  group by username,comment_text
  having comment_text is null
 ) as total_no_of_users_without_comments; 







 

