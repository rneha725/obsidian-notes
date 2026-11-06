Better to call it split master, multiple leader nodes.

When a network partition happens, some writes are going to the old leader. But the quorum decides a new master. Now there are two leaders. When the network partition is recovered, the old leader's data is lost and new leader is still the leader.

Similar problem comes when multiple processes can acquire locks and the one that locked can sleep, so the other one takes the lock and then processes, but the old one comes back.

### Solution:
Fencing token, ever increasing token number which lets the token with the highest value win.