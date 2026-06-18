# SINGLETON-TMG
SINGLETON TMG
<img width="1762" height="817" alt="image" src="https://github.com/user-attachments/assets/fdabf8e9-1800-4758-87fd-ebdcc5f9134d" />
***********************INTERFACE VIEW FOR SINGLETON ENTITY***********************************************************
@EndUserText.label: 'GL Accounts - Singleton (ZDDE-00102367)'
@AccessControl.authorizationCheck: #CHECK
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@VDM.viewType: #BASIC
@ObjectModel.semanticKey: ['SingletonID']
@UI: {
  headerInfo: {
    typeName: 'GLAccountsMaintanace'
    }
    }
define root view entity ZFI_I_GLAccounts_MNT_S
  as select from    I_Language
    left outer join zfi_t_glacct_mnt on 0 = 0
  composition [0..*] of ZFI_I_GLAccounts_MNT as _TableForGLAccounts
{
  key 1                                     as SingletonID,
      @UI.hidden: true
      max( zfi_t_glacct_mnt.lastchangedat ) as LastChangedAtMax,
      _TableForGLAccounts

}
where
  I_Language.Language = $session.system_language
  ********************************DRAFT QUERY***************************************************************************
  @AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Singleton Draft Query(ZDDE-00102368)'
@VDM.viewType: #BASIC
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZFI_I_GLACCT_SD_QUERY
  as select from zfi_t_glacct_d_s
{
  key singletonid                   as Singletonid,
      lastchangedatmax              as Lastchangedatmax,
      draftentitycreationdatetime   as Draftentitycreationdatetime,
      draftentitylastchangedatetime as Draftentitylastchangedatetime,
      draftadministrativedatauuid   as Draftadministrativedatauuid,
      draftentityoperationcode      as Draftentityoperationcode,
      hasactiveentity               as Hasactiveentity,
      draftfieldchanges             as Draftfieldchanges
}
*************************************DRAFT QUERY****************************************************************************
@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Draft Query for zfi_t_glacct_m_d'
@VDM.viewType: #BASIC
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZFI_I_GLACCT_MNT_DRFT_QUERY
  as select from zfi_t_glacct_m_d

{
  key uuid                          as uuid,
      bukrs                         as Bukrs,
      accounttype                   as Accounttype,
      glfrm                         as Glfrm,
      descfrm                       as Descfrm,
      glto                          as Glto,
      descto                        as Descto,
      createdby                     as Createdby,
      createdon                     as Createdon,
      lastmodifiedby                as Lastmodifiedby,
      lastchangedat                 as Lastchangedat,
      locallastchangedat            as Locallastchangedat,
      singletonid                   as Singletonid,
      draftentitycreationdatetime   as Draftentitycreationdatetime,
      draftentitylastchangedatetime as Draftentitylastchangedatetime,
      draftadministrativedatauuid   as Draftadministrativedatauuid,
      draftentityoperationcode      as Draftentityoperationcode,
      hasactiveentity               as Hasactiveentity,
      draftfieldchanges             as Draftfieldchanges
}
************************************CONSUMPTION VIEW FOR INTERFACE VIEW**************************************************************
@EndUserText.label: 'Table for GL Accounts(ZDDE 00102367) - M'
@AccessControl.authorizationCheck: #CHECK
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@VDM.viewType: #CONSUMPTION
@ObjectModel.semanticKey: ['Bukrs']
define view entity ZFI_C_GLAccounts_MNT
  as projection on ZFI_I_GLAccounts_MNT
{
  key uuid,
  Bukrs,
  AccountType,
  GlFrm,
  DescFrm,
  GlTo,
  DescTo,
  Createdby,
  Createdon,
  Lastmodifiedby,
  Lastchangedat,
  @Consumption.hidden: true
  Locallastchangedat,
  @Consumption.hidden: true
  SingletonID,
  /* Associations */
  _TableForGLAccounAll : redirected to parent ZFI_C_GLAccounts_MNT_S

}
***********************************************CONSUMPTION VIEW FOR SINGLETON INTERFACE VIEW*****************************************
@EndUserText.label: 'GL Accounts - Singleton (ZDDE-00102367) '
@AccessControl.authorizationCheck: #CHECK
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@VDM.viewType: #CONSUMPTION
@ObjectModel.semanticKey: [ 'SingletonID' ]
define root view entity ZFI_C_GLAccounts_MNT_S
  provider contract transactional_query
  as projection on ZFI_I_GLAccounts_MNT_S
{
  key SingletonID,
      @Consumption.hidden: true
      LastChangedAtMax,
      /* Associations */
      _TableForGLAccounts : redirected to composition child ZFI_C_GLAccounts_MNT
}
***********************************************************************************************************************
*************************************************METADATA EXTENSION******************************************************************
@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Sales Cutoff - GL Mapping', 
    typeNamePlural: 'Sales Cutoff - GL Mapping', 
    title: {
      type: #STANDARD, 
      label: 'Sales Cutoff - GL Mapping', 
      value: 'Bukrs'
    }
  }
}
annotate view ZFI_C_GLAccounts_MNT with
{
  @UI.identification: [ { position: 1 } ]
  @UI.lineItem: [ { position: 1 } ]
  @UI.facet: [ {
    id: 'ZFI_I_GLAccounts_MNT', 
    purpose: #STANDARD, 
    type: #IDENTIFICATION_REFERENCE, 
    label: 'Table for GL Accounts', 
    position: 1 } ]
  Bukrs;
  @UI.identification: [ { position: 2 } ]
  @UI.lineItem: [ { position: 2 } ]
  AccountType;
  @UI.identification: [ { position: 3 } ]
  @UI.lineItem: [ { position: 3 } ]
  GlFrm;
  @UI.identification: [ { position: 4 , label: 'DescFrm'} ]
  @UI.lineItem: [ { position: 4 , label: 'DescFrm'} ]
  DescFrm;
  @UI.identification: [ { position: 5 } ]
  @UI.lineItem: [ { position: 5 } ]
  GlTo;
  @UI.identification: [ { position: 6 , label: 'DescTo'} ]
  @UI.lineItem: [ { position: 6 , label: 'DescTo'} ]
  DescTo;
  @UI.identification: [{ position: 7 }]
  @EndUserText.label : 'Created By'
  Createdby;
  @UI.identification: [{ position: 8 }]
  @EndUserText.label : 'Created On'
  Createdon;
  @UI.identification: [{ position: 9 }]
  @EndUserText.label : 'Last Modified By'
  Lastmodifiedby;
  @UI.identification: [{ position: 10 }]
  @EndUserText.label : 'Changed At'
  Lastchangedat;
  @UI.identification: [{ position: 11 }]
  @EndUserText.label : 'Local Last Changed At'
  Locallastchangedat;
}
*********************************METADATA EXTENSION FOR SINGLETON ENTITY*********************************************************
@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Sales Cutoff - GL Mapping',
    typeNamePlural: 'Sales Cutoff - GL Mapping',
                     title:          { type: #STANDARD,
                                       label: 'Sales Cutoff - GL Mapping',
                                       value: 'SingletonID' } }
  }

annotate view ZFI_C_GLAccounts_MNT_S with
{
  @UI.facet: [
  {
    id: 'ZFI_I_GLAccounts_MNT',
    purpose: #STANDARD,
    type: #LINEITEM_REFERENCE,
    label: 'Table for GL Accounts',
    position: 10 ,
    targetElement: '_TableForGLAccounts'
  } ]
  @UI.lineItem: [ {
    position: 10
  } ]
  SingletonID;
}
******************************************BEHAVIOR DEFINITION*************************************************************************
managed implementation in class ZBP_I_GLACCOUNTS_MNT_S unique;
strict ( 2 );
with draft;

//Parent Root Entity
define behavior for ZFI_I_GLAccounts_MNT_S alias TableForGLAccounAll
with unmanaged save
draft table zfi_t_glacct_d_s query ZFI_I_GLACCT_SD_QUERY
lock master total etag LastChangedAtMax
authorization master ( global )

{
  create; update; delete;
  association _TableForGLAccounts { create; with draft; }
  field ( readonly ) SingletonID, LastChangedAtMax;

  draft action Edit;
  draft action Activate;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare;

}

//Child View Entity
define behavior for ZFI_I_GLAccounts_MNT alias TableForGLAccounts
persistent table zfi_t_glacct_mnt
draft table zfi_t_glacct_m_d query ZFI_I_GLACCT_MNT_DRFT_QUERY
lock dependent by _TableForGLAccounAll
authorization dependent by _TableForGLAccounAll
etag master Locallastchangedat
{
  field ( numbering : managed ) uuid;
  update ( features : instance );
  delete ( features : instance );

  field ( readonly ) SingletonID, Createdby, Createdon, Lastmodifiedby, Lastchangedat, Locallastchangedat;
  field ( readonly : update, mandatory : create ) Bukrs, AccountType, GlFrm;
  association _TableForGLAccounAll { with draft; }
  validation _validatekey on save { create; update; field Bukrs, AccountType, GlFrm; }

  mapping for ZFI_T_GLACCT_MNT
  {
    uuid = uuid;
    Bukrs = bukrs;
    AccountType = account_type;
    GlFrm = gl_frm;
    DescFrm = desc_frm;
    GlTo = gl_to;
    DescTo = desc_to;
    Createdby = createdby;
    Createdon = createdon;
    Lastmodifiedby = lastmodifiedby;
    Lastchangedat = lastchangedat;
    Locallastchangedat = locallastchangedat;
  }
}
********************************************BEHAVIOR CONSUMPTION********************************************************************
projection;
strict ( 2 );
use draft;

define behavior for ZFI_C_GLAccounts_MNT_S alias TableForGLAccounAll

{
  use action Edit;
  use action Activate;
  use action Discard;
  use action Resume;
  use action Prepare;

  use association _TableForGLAccounts { create; with draft; }
}

define behavior for ZFI_C_GLAccounts_MNT alias TableForGLAccounts

{
  use update;
  use delete;

  use association _TableForGLAccounAll { with draft; }
}
****************************************************************************************************************************************
***************************************************BEHAVIOR POOL CLASS*****************************************************************
**********************************************************************
* Development ID : ZDDE-00102368                                      *
* Functional Spec: ZFSE-00102367                                      *
*---------------------------------------------------------------------*
* Class method Def and Impl for behavior def. ZFI_I_GLAccounts_MNT_S  *
*---------------------------------------------------------------------*
* Change Log:                                                         *
*---------------------------------------------------------------------*
* Init     |  Name     |     Date   |     CR       |  Text            *
*---------------------------------------------------------------------*
* CHINTRE3  Reshma Ch    02-Nov-2025  CR: 1100276898  Initial version *
*---------------------------------------------------------------------*
CLASS lhc_tableforglaccounall DEFINITION INHERITING FROM cl_abap_behavior_handler FINAL.

  PUBLIC SECTION.

    CONSTANTS: gc_msg_id TYPE arbgb     VALUE 'ZFSE_00102367',
               gc_001    TYPE symsgno   VALUE '001',
               gc_actvt  TYPE fieldname VALUE 'ACTVT',
               gc_02     TYPE activ_auth VALUE '02'.

  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR tableforglaccounall RESULT result.

    METHODS is_update_granted
      RETURNING VALUE(update_granted) TYPE abap_bool.

ENDCLASS.

CLASS lhc_tableforglaccounall IMPLEMENTATION.

  METHOD get_global_authorizations.

    IF requested_authorizations-%update EQ if_abap_behv=>mk-on.
      IF is_update_granted( ) = abap_true.
        result-%update = if_abap_behv=>auth-allowed.
      ELSE.
        result-%update = if_abap_behv=>auth-unauthorized.
        result-%action-edit = if_abap_behv=>fc-o-disabled.
        APPEND VALUE #( %msg    = new_message( id      = gc_msg_id
                                               number  = gc_001
                                               severity = if_abap_behv_message=>severity-error )
                        %global = if_abap_behv=>mk-on
                      ) TO reported-tableforglaccounall.
      ENDIF.
    ENDIF.


  ENDMETHOD.

  METHOD is_update_granted.

* For Creation / Change Activity code "02" is being used.
    AUTHORITY-CHECK OBJECT 'S_TABU_NAM'
            ID gc_actvt FIELD gc_02
            ID 'TABLE' FIELD 'ZFI_T_GLACCT_MNT'.
    update_granted = COND #( WHEN sy-subrc = 0 THEN abap_true ELSE abap_false ).

  ENDMETHOD.

ENDCLASS.

CLASS lhc_tableforglaccounts DEFINITION INHERITING FROM cl_abap_behavior_handler FINAL.

  PUBLIC SECTION.
    CONSTANTS: gc_msg_id TYPE arbgb     VALUE 'ZFSE_00102367',
               gc_003    TYPE symsgno   VALUE '003',
               gc_bukrs  TYPE fieldname VALUE 'BUKRS',
               gc_actvt  TYPE fieldname VALUE 'ACTVT',
               gc_02     TYPE activ_auth VALUE '02',
               gc_06     TYPE activ_auth VALUE '06'.

  PRIVATE SECTION.

    METHODS get_instance_features FOR INSTANCE FEATURES
      IMPORTING keys REQUEST requested_features FOR tableforglaccounts RESULT result.

    METHODS _validatekey FOR VALIDATE ON SAVE
      IMPORTING keys FOR tableforglaccounts~_validatekey.

ENDCLASS.

CLASS lhc_tableforglaccounts IMPLEMENTATION.

  METHOD get_instance_features.

    DATA: lv_del_auth TYPE abap_bool VALUE abap_false,
          lv_upd_auth TYPE abap_bool VALUE abap_false.
*- Reading the Entries of the entity
    READ ENTITIES OF zfi_i_glaccounts_mnt_s IN LOCAL MODE
    ENTITY tableforglaccounts
    ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_gl_acct)
    FAILED DATA(lt_failed).

*- Checking the authorization Object
    LOOP AT lt_gl_acct ASSIGNING FIELD-SYMBOL(<lfs_gl_acct>).

      lv_del_auth = lv_upd_auth = abap_false.
      IF requested_features-%delete = if_abap_behv=>mk-on.
        AUTHORITY-CHECK OBJECT 'F_BKPF_BUK'
                        ID gc_actvt FIELD gc_06
                        ID gc_bukrs
                        FIELD <lfs_gl_acct>-bukrs.
        IF sy-subrc EQ 0.
          lv_del_auth = abap_true.
        ENDIF.
      ENDIF.

      IF  requested_features-%update = if_abap_behv=>mk-on.
        AUTHORITY-CHECK OBJECT 'F_BKPF_BUK'
                        ID gc_actvt FIELD gc_02
                        ID gc_bukrs
                        FIELD  <lfs_gl_acct>-bukrs.
        IF sy-subrc EQ 0.
          lv_upd_auth = abap_true.
        ENDIF.
      ENDIF.

*- Appending the value if the conditions are failed or success
      APPEND VALUE #( LET upd_auth = COND #( WHEN lv_upd_auth = abap_true THEN if_abap_behv=>auth-allowed
                                                   ELSE if_abap_behv=>auth-unauthorized )
                          del_auth = COND #( WHEN lv_del_auth = abap_false THEN if_abap_behv=>auth-allowed
                                                   ELSE if_abap_behv=>auth-allowed )

                            IN
                             %tky = <lfs_gl_acct>-%tky
                             %update                = upd_auth
                             %delete                = del_auth
                          ) TO result.

    ENDLOOP.
  ENDMETHOD.

  METHOD _validatekey.

    DATA(lt_draft) = keys.
    IF lt_draft IS NOT INITIAL.

      SELECT FROM
      zfi_t_glacct_m_d AS dt
      INNER JOIN @lt_draft AS k
      ON k~uuid = dt~uuid
      LEFT OUTER JOIN zfi_t_glacct_mnt AS ct
      ON ct~bukrs = dt~bukrs
      AND ct~account_type = dt~accounttype
      AND ct~gl_frm = dt~glfrm
      FIELDS DISTINCT dt~uuid,
                      dt~bukrs,
                      dt~accounttype,
                      dt~glfrm,
                      ct~bukrs AS act_bukrs
      INTO TABLE @DATA(lt_check).

      LOOP AT lt_check ASSIGNING FIELD-SYMBOL(<lfs_conflict>).
        IF <lfs_conflict>-act_bukrs IS NOT INITIAL.

          DATA(lv_tky) = VALUE #( lt_draft[ KEY entity uuid = <lfs_conflict>-uuid ]-%tky OPTIONAL ).

          APPEND VALUE #( %tky = lv_tky ) TO failed-tableforglaccounts.
          APPEND VALUE #(
            %tky = lv_tky
            %msg = new_message(
                    id       = gc_msg_id
                    number   = gc_003
                    v1       = <lfs_conflict>-bukrs
                    v2       = <lfs_conflict>-accounttype
                    v3       = <lfs_conflict>-glfrm
                    severity = if_abap_behv_message=>severity-error )
          ) TO reported-tableforglaccounts.

        ENDIF.
      ENDLOOP.
    ENDIF.

  ENDMETHOD.

ENDCLASS.

CLASS lsc_zfi_i_glaccounts_mnt_s DEFINITION INHERITING FROM cl_abap_behavior_saver FINAL.
  PROTECTED SECTION.

    METHODS save_modified REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_zfi_i_glaccounts_mnt_s IMPLEMENTATION.

  METHOD save_modified.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.
**********************************************ACCESS CONTROL******************************************************************
@EndUserText.label: 'Access Control (ZDDE-00102368)'
@MappingRole: true
define role ZFI_I_GLACCOUNTS_MNT_S {
    grant
        select
            on
                ZFI_I_GLACCOUNTS_MNT_S
                    where
                         ( ) = aspect pfcg_auth(Z_CDS_VIEW, DDLNAME = 'ZFI_I_GLACCOUNTS_MNT_S', ACTVT = '03');
                        
}

  
