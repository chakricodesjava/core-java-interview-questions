QA EXCEPTIONS & ERROR HANDLING 

Q41. Checked vs Unchecked
- Key: Checked = declare/catch; Unchecked = programmer bugs
- Pitfall: Overusing checked exceptions
- Demo: read(Path) throws IOException vs IllegalArgumentException for bad args

Q42. throw vs throws
- Key: throw (action) vs throws (contract)
- Pitfall: Throw checked without declaring
- Demo: save(Path) throws IOException; setLimit(int) throws IllegalArgumentException

Q43. try-with-resources
- Key: Auto-close AutoCloseable, reverse order; suppressed exceptions
- Pitfall: Ignoring getSuppressed()
- Demo: InputStream → OutputStream; Java 9 ARM with existing variable

Q44. finally Block Behavior
- Key: finally runs even on return; avoid return in finally
- Pitfall: Swallowing primary exception in finally
- Demo: f() returns 2; try + finally throwing second exception

Q45. Custom Exceptions
- Key: Extend Exception or RuntimeException; include cause constructor
- Pitfall: Deep hierarchies, empty catch blocks
- Demo: PaymentFailedException, InvalidOrderStateException

Q46. Error vs Exception
- Key: Errors are serious; don’t catch Throwable
- Pitfall: Masking OOME/StackOverflowError
- Demo: catch(Exception) vs catch(Throwable)

Q47. Re-throw vs Wrap
- Key: Wrap to add context; keep cause; rethrow when appropriate
- Pitfall: Losing stack trace or cause
- Demo: wrap SQLException → PaymentFailedException; good vs bad rethrow

Q48. StackOverflowError vs OutOfMemoryError
- Key: Recursion vs memory exhaustion; flags -Xss/-Xmx
- Pitfall: Confusing types of OOME
- Demo: recurse(); list of big byte[]

Q49. Suppressed Exceptions
- Key: Close-time exceptions attached to primary
- Pitfall: Not logging suppressed
- Demo: MyCloseable.close() throws; read e.getSuppressed()

Q50. When to Use RuntimeException
- Key: Precondition/invariant violations; not control flow
- Pitfall: Using RuntimeException for expected recoverable cases
- Demo: process(Order) validates inputs and state

Extras for video:
- Start with 5–10s hook; diagram flows; show suppressed exceptions; end with a rule-of-thumb.

