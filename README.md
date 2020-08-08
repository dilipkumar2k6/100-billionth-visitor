# 100-billionth-visitor
# Design constraints 
- 50,000 user per seconds as peak traffic
# Design
- Use N server
- Each server will write counter locally
- Periodically each server will send local counter to aggregator server
- Aggregator server will keep global counter value and return to each server
- Each server will see if 1billion/N counter is achived
    - If yes then check the central co-ordination service
    - If no one has identify 1 billion then update the central server and send to user
    