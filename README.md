.SYNOPSIS
    Ce script PowerShell applique des rôles Azure spécifiques au groupe de sécurité "Foreign Principal"
    pour l'ensemble des abonnements Azure présents dans un Azure plan. Il supprime également le rôle "Owner"
    pour ce groupe après l'attribution des rôles moins privilégiés.

.DESCRIPTION
    Ce script effectue les actions suivantes :
    1. Configure le chemin du fichier de log et du fichier csv de controle.
    2. Vérification des droits administrateur et relance.
    (Option, commenté L105) 3. Détection de la version de PowerShell 7+ et mise à jour si nécessaire via winget. 
    4. Vérifie et définit la politique d'exécution PowerShell sur RemoteSigned.
    (Option, commenté L155) 5. Installe et importe le module Az si nécessaire.
    6. Demande une connexion à Azure via l'authentification de l'appareil.
    7. Récupère tous les abonnements Azure associés au compte connecté.
    8. Définit l'ID de l'objet du groupe de sécurité "Foreign Principal".
    9. Applique les rôles spécifiés (Reader, Billing Reader, Monitoring Reader, Quota Request Operator,
       Support Request Contributor, Reservation Purchaser) au niveau du groupe de management racine ou par abonnement.
    10. Applique le rôle "Cost Management Contributor" spécifiquement à chaque abonnement.
    11. Vérifie si le groupe "Foreign Principal" a le rôle "Owner" sur chaque abonnement ou groupe de management racine,
        puis supprime ces attributions de rôle.
    12. Exporte un rapport CSV détaillé incluant les rôles 'Owner' trouvés et supprimés.
    13. Déconnecte le compte Azure.

.NOTES
    Prérequis :
    - Le script doit être exécuté par un utilisateur disposant des droits suffisants (ex: Global Admin ou User Access Administrator)
      pour attribuer et supprimer des rôles au niveau du groupe de management racine et/ou des abonnements.
    - La relation d'administration entre le fournisseur CSP et le client final doit être active.
    - L'ID de l'objet du groupe de sécurité "Foreign Principal" doit être défini dans la variable `$ForeignPrincipalObjectId`.
    - Nécessite la connectivité Internet.
    - il recommandé d'utiliser Powershell en version 7.x & + 
    - winget doit être installé pour la mise à jour automatique de PowerShell.
    - L'UAC peut demander une confirmation lors de l'exécution.


=====================================================================================================================================================

.SYNOPSIS
This PowerShell script applies specific Azure roles to the “ForeignPrincipal” security group for all Azure subscriptions in an Azure plan. It also 
removes the “Owner” role for this group after assigning less privileged roles.

DESCRIPTION
This script performs the following actions:
	1. Configures the path for the log file and control CSV file.
	2. Verifies administrator rights and restarts.
	(Option, commented L105) 3. Detects PowerShell 7+ version and updates if necessary via winget. 
  4. Checks and sets the PowerShell execution policy to RemoteSigned.
  (Option, commented L155) 5. Installs and imports the Az module if necessary.
  6. Requests a connection to Azure via device authentication.
  7. Retrieves all Azure subscriptions associated with the connected account.
	8. Sets the object ID of the “Foreign Principal” security group.
	9. Applies the specified roles (Reader, Billing Reader, Monitoring Reader, Quota Request Operator,Support Request Contributor, Reservation Purchaser) 
     at the root management group or subscription level.
	10. Apply the “Cost Management Contributor” role specifically to each subscription.
	11. Check whether the “Foreign Principal” group has the “Owner” role on each subscription or root management group,
		  then remove these role assignments.
	12. Export a detailed CSV report including the ‘Owner’ roles found and removed.
  13. Disconnect the Azure account.

NOTES
Prerequisites:
	- The script must be run by a user with sufficient rights (e.g., Global Admin or User Access Administrator) to assign and remove roles at the root management group and/or subscription level.
  - The administrative relationship between the CSP provider and the end customer must be active.
	- The ID of the “Foreign Principal” security group object must be defined in the `$ForeignPrincipalObjectId` variable.
	- Requires Internet connectivity.
  - It is recommended to use Powershell version 7.x & +
	- winget must be installed for automatic PowerShell updates.
	- UAC may request confirmation during execution.
