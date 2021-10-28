from collections import namedtuple
from typing import List
import re
import os
import csv
import pandas as pd
import datetime
import time

repetidor = True
registro = namedtuple("Registro", ("descripcion","cantidad","fecha","precio", "total", "total_iva"))
objecto_registrado = {}
registado_completo = {}
separador = ("-" * 20) + "\n"
total_monto = 0


while repetidor:
    print("MENU REGISTRO DE PERSONA")
    print("1: Agregar un registro ")
    print("2: Registro especifico")
    print("3: exportar a csv")
    print("4: salir")

    print(separador)

    resp = input("Ingrese una opción: ")
    
    if resp == "1":
        Registros_Marcados = []
        otro_registro = "S"
        folio = input("Marque su folio por favor: ")
        fecha = input("Dime una fecha: ")
        procesado = datetime.datetime.strptime(fecha, "%d/%m/%Y").date()
        while otro_registro.upper()=="S":
            descripcion = input("Marque el producto que desea llevarse: ")
            cantidad = int(input("La cantidad del producto que desea llevar: "))
            precio = int(input("El precio de sus productos: "))
            
            total = (cantidad * precio )
            total_monto = total_monto + total
            iva = (total_monto * 0.16)
            total_iva = total_monto + iva
            
            print(separador)
            Registrado = registro(descripcion,cantidad, fecha, precio, total, total_iva)
            Registros_Marcados.append(Registrado)
            print("¿Desea Hacer otro registro?  [S/N]")
            otro_registro=input()
            
            registado_completo[folio] = [Registros_Marcados,descripcion,cantidad, fecha, precio, total, total_iva]
    

    elif resp == "2":
        buscar = input("Ingreso folio es: ")
        if buscar in registado_completo.keys():
            print(f"La folio es: {buscar} ")
            print(f"La fecha es: {registado_completo[buscar][3]} ")
            for buscar_producto in registado_completo[buscar][0]:
                print(f"la cantidad que se llevara es: {buscar_producto.cantidad}")
                print(f"La descripcion del producto es: {buscar_producto.descripcion}")
                print(f"El precio del producto es: {buscar_producto.precio}")
            print(f"El monto del producto es: {registado_completo[buscar][2]} ")
            print(f"El iva sera: {registado_completo[buscar][1]} ") 
            print(separador)     

   
        else:
            print("No se encontro registro ")
            
    elif resp == "3":
        List = [folio, fecha, descripcion, cantidad, fecha, precio, total, total_iva]
        print (str(List[0]) + "\t" + str(List[1]) + "\t" + str(List[2]))
        with open("Registrossubidos.csv", "w", newline="") as cosas_registradas:
            completos = csv.writer(cosas_registradas)
            completos.writerow(("descripcion","cantidad","precio", "total", "total_iva"))
            completos.writerows([(descripcion, cantidad, precio, total, total_iva) for folio in registado_completo.items()])
            print(f"---Grabado exitoso en-----> {os.getcwd()}")
        
    elif resp == "4":
        repetidor = False        
    else:
        print("Ingrese un numero valido")
