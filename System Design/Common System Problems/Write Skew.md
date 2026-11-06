We check some condition and update different rows.

Example:
- Check if the inventory is available and create a release entry if inventory is available.
- Two transactions can read and write without any issue, resulting in overselling.

Pattern:
- Read some rows and check a condition.
- Write different rows based on the condition.

How to prevent it
- **Serializable isolation.** The database detects the conflict and aborts one transaction. Here the database keeps tracking the ongoing transactions. This is expensive and better to use pessimistic or optimistic locking. Also, major disadvantage is that most NoSQL do not support these transactions.
- **Lock the rows you read.** Use `SELECT ... FOR UPDATE` so the second transaction waits.
- **Materialize the conflict.** If there’s no row to lock (e.g., checking that something _doesn’t_ exist), create a row that both transactions must touch, like a stock counter row.
- **Use a constraint.** Where possible, let the database enforce the rule directly.
