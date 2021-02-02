```
use EformDB;
declare @MyCursor Cursor, @userid uniqueidentifier, @visitid uniqueidentifier;
BEGIN
    SET @MyCursor = CURSOR FOR
    select Users.Id, Visits.Id from Visits 
	  where Visits.PrimaryNurseId is not null and Visits.CreatedAt < '2020-06-18 00:00:00';

    OPEN @MyCursor;
    FETCH NEXT FROM @MyCursor into @userid, @visitid;

    WHILE @@FETCH_STATUS = 0
    BEGIN
      update OPDs set PrimaryNurseId = @userid where Id = @visitid;
      FETCH NEXT FROM @MyCursor into @userid, @visitid;
    END; 

    CLOSE @MyCursor ;
    DEALLOCATE @MyCursor;
END;
```
