# sqlcoin

### Tokens

```
create table sqlcoin (
   owner varchar(64),
   amount decimal(10, 6),
   constraint owner_ref
    foreign key (nickname)
    references users(nickname),
   constraint amount_chain collectible(amount)
);

money transfer transaction:

update table sqlcoin set amount=amount+@1 where owner=‘bob’;
update table sqlcoin set amount=amount-@1 where owner=‘alice’;
signed by ‘alice’;
```

### Assets

```
create table 

```
