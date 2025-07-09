# 1. Nombres que comienzan con 'A'
db.collection.find({ nombre: { $regex: /^A/, $options: 'i' } })

# 2. Apellidos que terminan con 'ez'
db.collection.find({ apellido: { $regex: /ez$/, $options: 'i' } })

# 3. Correos que contienen 'gmail'
db.collection.find({ correo: { $regex: /gmail/, $options: 'i' } })

# 4. Correos que terminan en '.com'
db.collection.find({ correo: { $regex: /\.com$/, $options: 'i' } })

# 5. Ciudades que contienen una 'o'
db.collection.find({ ciudad: { $regex: /o/, $options: 'i' } })

# 6. Profesiones que comienzan con 'E'
db.collection.find({ profesion: { $regex: /^E/, $options: 'i' } })

# 7. Nombres que contienen una vocal
db.collection.find({ nombre: { $regex: /[aeiou]/, $options: 'i' } })

# 8. Apellidos con doble letra 'r'
db.collection.find({ apellido: { $regex: /rr/, $options: 'i' } })

# 9. Correos con números
db.collection.find({ correo: { $regex: /\d/ } })

# 10. Profesiones que contienen 'Manager'
db.collection.find({ profesion: { $regex: /Manager/, $options: 'i' } })

# 11. Nombres con exactamente 4 letras
db.collection.find({ nombre: { $regex: /^.{4}$/, $options: 'i' } })

# 12. Apellidos con más de 6 letras
db.collection.find({ apellido: { $regex: /^.{7,}$/, $options: 'i' } })

# 13. Correos que comienzan con 'l'
db.collection.find({ correo: { $regex: /^l/, $options: 'i' } })

# 14. Ciudades que terminan con vocal
db.collection.find({ ciudad: { $regex: /[aeiou]$/, $options: 'i' } })

# 15. Teléfonos con guiones
db.collection.find({ telefono: { $regex: /-/ } })

# 16. Profesiones que contienen 'Engineer'
db.collection.find({ profesion: { $regex: /Engineer/, $options: 'i' } })

# 17. Nombres con letras mayúsculas
db.collection.find({ nombre: { $regex: /[A-Z]/ } })

# 18. Apellidos que contienen 'z'
db.collection.find({ apellido: { $regex: /z/, $options: 'i' } })

# 19. Correos con subdominio
db.collection.find({ correo: { $regex: /^[^@]+@[^@]+\.[^@]+$/, $options: 'i' } })

# 20. Ciudades que comienzan con consonante
db.collection.find({ ciudad: { $regex: /^[^aeiou]/, $options: 'i' } })

# 21. Nombres que terminan con 'n'
db.collection.find({ nombre: { $regex: /n$/, $options: 'i' } })

# 22. Profesiones que terminan en 'ist'
db.collection.find({ profesion: { $regex: /ist$/, $options: 'i' } })

# 23. Apellidos con 'a' seguida de consonante
db.collection.find({ apellido: { $regex: /a[^aeiou]/, $options: 'i' } })

# 24. Correos que no contienen números
db.collection.find({ correo: { $not: { $regex: /\d/ } } })

# 25. Ciudades con doble vocal
db.collection.find({ ciudad: { $regex: /[aeiou]{2}/, $options: 'i' } })

# 26. Profesiones con espacios
db.collection.find({ profesion: { $regex: /\s/ } })

# 27. Nombres con letra repetida
db.collection.find({ nombre: { $regex: /(.).*\1/, $options: 'i' } })

# 28. Apellidos sin 'e'
db.collection.find({ apellido: { $not: { $regex: /e/, $options: 'i' } } })

# 29. Profesiones en plural (terminan en 's')
db.collection.find({ profesion: { $regex: /s$/, $options: 'i' } })

# 30. Teléfonos que comienzan con paréntesis
db.collection.find({ telefono: { $regex: /^\(/ } })
