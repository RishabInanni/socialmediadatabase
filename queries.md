# Queries
 
# Top 3 users according to created date.
  select * from users
  order by created_at
  limit 3;

# Days in which most user registed in app

select extract(dow from created_at) as "day of week",count(*)
from users
group by 1
order by 2 desc;

select to_char(created_at,'Dy') as day,
count(*) as total
from users
group by day
order by total desc;



# To find inactive user
select username from users
left join photos on users.id=photos.user_id
where photos.id is null;

# How many times the average user posts
select round((select count(*) from photos)/(select count(*)from users),2);

# To find most active user

select users.username ,count(photos.image_url) from users
join photos on users.id=photos.user_id
group by users.id
order by 2 desc;


# Most used hastags

select tag_name ,count(tag_name) as total
from tags
join photo_tags on tags.id=photo_tags.tag_id
group by tags.id
order by total desc;

# No of users who posted atleast once

select count(distinct(users.id)) as total_no_of_usr_with_post
from users
join photos on users.id=photos.user_id;

  
#Users who have not commented 
select username,comment_text
from users
left join comments on users.id=comments.user_id
group by users.id,comment_text
having comment_text is null;
 
 
# No of people who never commented on any of the post
 select count(*) from
 ( select username,comment_text
  from users 
  left join comments on users.id=comments.user_id
  group by username,comment_text
  having comment_text is null
 ) as total_no_of_users_without_comments; 
