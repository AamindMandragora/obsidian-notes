`int sigprocmask(int how, const sigset_t *set, sigset_t *oldset)` allows us to alter the signal mask. `how` can either be `SIG_BLOCK`, taking the union of the old and new sets, `SIG_UNBLOCK`, taking the difference of the old and new sets, or `SIG_SETMASK`, which assigns the old set to the new set.

We must remember to always call either `sigfillset` or `sigemptyset` to initialize it before doing anything else. If we block a signal with either `sigprocmask` or `pthread_sigmask`, then the handler registered with `sigaction` isn't delivered unless `sigwait` is called on the signal.
# `sigwait`

`sigwait` is used to synchronously wait and read one pending signal instead of handling it in a callback. We can use a mask to prevent any signals we want to `sigwait` on from being delivered. Writing a custom signal handling thread means we can use many more functions safely, as seen below:

```c
static sigset_t signal_mask;

int main(int argc, char *argv[]) {
	pthread_t sig_thr_id;
	sigemptyset(&signal_mask);
	sigaddset(&signal_mask, SIGINT);
	sigaddset(&signal_mask, SIGTERM);
	pthread_sigmask(SIG_BLOCK, &signal_mask, NULL);
	
	pthread_create(&sig_thr_id, NULL, signal_thread, NULL);
	
	/* other code... */
}

void *signal_thread(void *arg) {
	int sig_caught;
	sigwait(&signal_mask, &sig_caught);
	switch (sig_caught) {
		case SIGINT:
			// ...
			break;
		case SIGTERM:
			// ...
			break;
		default:
			// unexpected signal
			break;
	}
}
```