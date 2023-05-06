# Overall Database Structure

## General Tables

**users**
- user_id (PK, INT, AUTO_INCREMENT)
- username (VARCHAR)
- password (VARCHAR)
- first_name (VARCHAR)
- last_name (VARCHAR)
- email (VARCHAR)
- role (ENUM: 'admin', 'manager', 'employee')
- location_id (INT, FK)
- created_at (DATETIME)
- updated_at (DATETIME)

## Point of Sale

**currencies**
- currency_id (PK, INT, AUTO_INCREMENT)
- currency_code (CHAR(3), UNIQUE)
- currency_name (VARCHAR)

**products**
- product_id (PK, INT, AUTO_INCREMENT)
- product_name (VARCHAR)
- product_description (TEXT)
- category_id (INT, FK)
- currency_id (INT, FK)
- price (DECIMAL)
- image_url (VARCHAR)
- barcode (VARCHAR)
- created_at (DATETIME)
- updated_at (DATETIME)

**categories**
- category_id (PK, INT, AUTO_INCREMENT)
- category_name (VARCHAR)
- description (TEXT)
- parent_category_id (INT, FK, nullable)

**inventory**
- inventory_id (PK, INT, AUTO_INCREMENT)
- product_id (INT, FK)
- location_id (INT, FK)
- quantity (INT)
- reorder_level (INT)
- created_at (DATETIME)
- updated_at (DATETIME)

**locations**
- location_id (PK, INT, AUTO_INCREMENT)
- location_name (VARCHAR)
- address (TEXT)
- phone (VARCHAR)

**tables**
- table_id (PK, INT, AUTO_INCREMENT)
- location_id (INT, FK)
- table_number (INT)
- capacity (INT)
- status (ENUM: 'available', 'occupied', 'reserved', 'maintenance')
- x_position (INT)
- y_position (INT)

**table_layouts**
- layout_id (PK, INT, AUTO_INCREMENT)
- location_id (INT, FK)
- layout_name (VARCHAR)
- layout_image (VARCHAR)

**seats**
- seat_id (PK, INT, AUTO_INCREMENT)
- table_id (INT, FK)
- seat_number (INT)

**sales**
- sale_id (PK, INT, AUTO_INCREMENT)
- user_id (INT, FK)
- table_id (INT, FK)
- payment_method (ENUM: 'cash', 'credit_card', 'crypto')
- total_amount (DECIMAL)
- sale_date (DATETIME)
- check_status (ENUM: 'open', 'closed', 'split')

**sale_items**
- sale_item_id (PK, INT, AUTO_INCREMENT)
- sale_id (INT, FK)
- product_id (INT, FK)
- quantity (INT)
- price (DECIMAL)

**customers**
- customer_id (PK, INT, AUTO_INCREMENT)
- first_name (VARCHAR)
- last_name (VARCHAR)
- email (VARCHAR)
- phone (VARCHAR)
- address (TEXT)
- created_at (DATETIME)
- updated_at (DATETIME)

**crypto_payments**
- payment_id (PK, INT, AUTO_INCREMENT)
- sale_id (INT, FK)
- crypto_address (VARCHAR)
- amount (DECIMAL)
- transaction_id (VARCHAR)
- status (ENUM: 'pending', 'confirmed', 'failed')
- created_at (DATETIME)
- updated_at (DATETIME)

**checks**
- check_id (PK, INT, AUTO_INCREMENT)
- sale_id (INT, FK)
- seat_id (INT, FK)
- total_amount (DECIMAL)
- check_status (ENUM: 'open', 'closed', 'paid')
- created_at (DATETIME)
- updated_at (DATETIME)

**check_items**
- check_item_id (PK, INT, AUTO_INCREMENT)
- check_id (INT, FK)
- sale_item_id (INT

## Kitchen

ingredients

ingredient_id (PK, INT, AUTO_INCREMENT)
ingredient_name (VARCHAR)
ingredient_description (TEXT)
unit_of_measure (VARCHAR)
recipes

recipe_id (PK, INT, AUTO_INCREMENT)
product_id (INT, FK)
recipe_name (VARCHAR)
recipe_description (TEXT)
prep_time (TIME)
cook_time (TIME)
total_time (TIME)
yield (INT)
recipe_steps

step_id (PK, INT, AUTO_INCREMENT)
recipe_id (INT, FK)
step_number (INT)
step_description (TEXT)
duration (TIME)
recipe_ingredients

recipe_ingredient_id (PK, INT, AUTO_INCREMENT)
recipe_id (INT, FK)
ingredient_id (INT, FK)
quantity (DECIMAL)
ingredient_inventory

kitchen_order_id (PK, INT, AUTO_INCREMENT)
sale_id (INT, FK)
product_id (INT, FK)
quantity (INT)
order_status (ENUM: 'queued', 'preparing', 'ready', 'served', 'cancelled')
created_at (DATETIME)
updated_at (DATETIME)

## inventory

ingredient_inventory_id (PK, INT, AUTO_INCREMENT)
ingredient_id (INT, FK)
location_id (INT, FK)
quantity (DECIMAL)
reorder_level (DECIMAL)
created_at (DATETIME)
updated_at (DATETIME)
kitchen_orders

**purchase_orders**
- purchase_order_id (PK, INT, AUTO_INCREMENT)
- supplier_id (INT, FK)
- location_id (INT, FK)
- user_id (INT, FK)
- order_date (DATETIME)
- delivery_date (DATETIME, nullable)
- status (ENUM: 'draft', 'submitted', 'received', 'cancelled')
- created_at (DATETIME)
- updated_at (DATETIME)

**purchase_order_items**
- order_item_id (PK, INT, AUTO_INCREMENT)
- purchase_order_id (INT, FK)
- ingredient_id (INT, FK)
- quantity (DECIMAL)
- unit_price (DECIMAL)
- created_at (DATETIME)
- updated_at (DATETIME)

**suppliers**
- supplier_id (PK, INT, AUTO_INCREMENT)
- supplier_name (VARCHAR)
- contact_person (VARCHAR)
- phone (VARCHAR)
- email (VARCHAR)
- address (TEXT)
- created_at (DATETIME)
- updated_at (DATETIME)

**losses**
- loss_id (PK, INT, AUTO_INCREMENT)
- ingredient_id (INT, FK)
- location_id (INT, FK)
- user_id (INT, FK)
- loss_type (ENUM: 'waste', 'damage', 'spoilage', 'other')
- quantity (DECIMAL)
- reason (TEXT)
- loss_date (DATETIME)
- created_at (DATETIME)
- updated_at (DATETIME)

## Equipment (Including printers, automated kitchen tools, etc.)

**equipment_categories**
- category_id (PK, INT, AUTO_INCREMENT)
- category_name (VARCHAR)
- description (TEXT)
- parent_category_id (INT, FK, nullable)

**equipment**
- equipment_id (PK, INT, AUTO_INCREMENT)
- equipment_name (VARCHAR)
- category_id (INT, FK)
- description (TEXT)
- model (VARCHAR)
- serial_number (VARCHAR)
- manufacturer (VARCHAR)
- purchase_date (DATETIME)
- warranty_expiration (DATETIME, nullable)
- location_id (INT, FK)
- status (ENUM: 'operational', 'maintenance', 'out_of_order')
- created_at (DATETIME)
- updated_at (DATETIME)

**maintenance_records**
- maintenance_id (PK, INT, AUTO_INCREMENT)
- equipment_id (INT, FK)
- user_id (INT, FK)
- maintenance_type (ENUM: 'preventive', 'corrective', 'upgrade')
- description (TEXT)
- start_date (DATETIME)
- end_date (DATETIME, nullable)
- created_at (DATETIME)
- updated_at (DATETIME)
