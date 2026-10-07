[[LINUX/]]


```
# 1. Display system logs
log show

# 2. Display logs from the last 1 hour
log show --last 1h

# 3. Display the last 20 log entries
log show --last 1h | tail -20

# 4. Search for an exact error message
log show --last 1h | grep -F "Operation not permitted"

# 5. Case-insensitive search
log show --last 1h | grep -Fi "Operation not permitted"

# 6. Search for error OR failed using regular expressions
log show --last 1h | grep -Ei "error|failed"

# 7. Display only 10 matching messages
log show --last 1h | grep -Ei "error|failed" | head -10

# 8. Count error messages
log show --last 1h | grep -Ei "error" | wc -l

# 9. Count error OR failed messages
log show --last 1h | grep -Ei "error|failed" | wc -l

# 10. Save filtered logs into a file
log show --last 1h | grep -Ei "error|failed" > errors.txt

# 11. Save only 10 matching messages
log show --last 1h | grep -Ei "error|failed" | head -10 > errors.txt

# 12. Display the saved log file
cat errors.txt

# 13. Display live logs
log stream

# 14. Search live logs for errors
log stream | grep -Ei "error|failed"

```

