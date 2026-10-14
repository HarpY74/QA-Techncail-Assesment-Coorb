### 2.2 Exploration Approach and Next Steps

**Approach Taken**

I started by reviewing the core account and money movement workflows, prioritizing areas where defects could directly affect financial integrity. First, I checked the Accounts Overview to understand the initial account balance, then opened a new account to observe how funds were allocated between accounts. Next, I tested Transfer Funds, focusing on whether the application prevented transfers exceeding the available balance. This revealed that the application accepted a transfer of $1,000 from an account containing $400, resulting in a negative balance of -$600. I then tested Bill Pay, including a zero-amount payment, which the application accepted and recorded in the transaction history.

Afterward, I explored Find Transactions, Request Loan, and Update Contact Info to identify additional functional issues and observe how transactions and profile changes were reflected in the application. I prioritized financial integrity and transaction validation over cosmetic issues, and selected two distinct findings for further investigation.

**Where I Would Look Next**

With two additional hours, I would investigate boundary-value scenarios for transfers and bill payments, including zero, negative, exact-balance, and over-balance amounts. I would compare account balances against transaction history to identify inconsistencies or duplicate transactions. I would also examine loan approval and disbursement behavior, verify whether profile changes persist after logout and login, and test error handling for invalid inputs. Finally, I would capture supporting screenshots, document reproducible steps, and retest the identified issues to confirm their behavior and impact.
