# Social Media Database



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


![er diagram](https://github.com/RishabInanni/socialmediadatabase/assets/110304592/4a6518e2-1c21-4627-90fa-6582f99cce54)





 

