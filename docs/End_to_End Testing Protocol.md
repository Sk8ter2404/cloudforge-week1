# Week 3 Complete Testing Protocol
   
   ## Test 1: New Customer Registration
   1. Register new account with valid email
   2. Verify email received
   3. Click confirmation link
   4. Login successful
   
   ## Test 2: Order and Provision Micro VPS
   1. Browse products
   2. Select Micro VPS, Ubuntu 22.04
   3. Add to cart
   4. Proceed to checkout
   5. Enter test credit card: 4242 4242 4242 4242
   6. Complete payment
   7. Verify order confirmation page
   8. Check email for order confirmation
   9. Wait max 5 minutes
   10. Check email for VM credentials
   11. Verify credentials work (SSH test)
   
   ## Test 3: VM Management
   1. VM should be running after provisioning
   2. Check Proxmox - VM should exist
   3. SSH into VM successfully
   4. Run basic commands
   5. Verify specs match order (CPU, RAM, Disk)
   
   ## Test 4: Payment Failure Handling
   1. Try to order with declined card: 4000 0000 0000 0002
   2. Verify order fails gracefully
   3. Verify no VM is created
   4. Verify customer notified of failure
   
   ## Test 5: Multiple Orders
   1. Order 3 different VPS plans
   2. Verify all 3 provision successfully
   3. Verify all 3 get unique credentials
   4. Verify all 3 receive separate emails
   
   ## Test 6: Concurrent Orders (Stress Test)
   1. Have 4 team members order simultaneously
   2. Verify all orders process correctly
   3. Check for any race conditions
   4. Verify no duplicate VMIDs
   
   ## Test 7: Edge Cases
   1. Order with invalid email format
   2. Order with special characters in name
   3. Multiple rapid orders from same account
   4. Order during Proxmox maintenance
