> Niektóre polecenia nie są obsługiwane, jeśli aplikacja używa oprogramowania SQLite jako magazynu danych tożsamości. Ze względu na ograniczenia w aparacie bazy danych polecenia `Alter` zgłaszają następujący wyjątek:
>
> "System. NotSupportedException: Program SQLite nie obsługuje tej operacji migracji". 
>
> Aby to obejść, uruchom Code First migracji w bazie danych, aby zmienić tabele.
