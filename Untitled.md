```typescript
import { Fragment, useState } from "react";

import { useUser } from "@/api";

import {

RecurrenceItem,

useRecurrenceDetails,

} from "@/api/useRecurrenceDetails";

import { useMediaQuery } from "@/components/hooks/useMediaQuery";

import { Details } from "@/components/RecurrenceDetails";

import { getRecurrenceStatusByNumber } from "@/utils/getRecurrenceStatus";

import { formatCurrency, Hooks } from "@hyperlocal/banking-utility";

import { Drawer, Skeleton as VitalSkeleton } from "@hyperlocal/vital";

import { groupBy } from "lodash";

import { Outlet, useNavigate, useParams } from "react-router-dom";

import { mountRootParcel } from "single-spa";

import Parcel from "single-spa-react/parcel";

import { useUrlDisclosure } from "../hooks/useUrlDisclosure";

import * as Styles from "./InstallmentsList.styles";

  

function formatDateToInstallmentDisplay(date: Date) {

const day = date.getDate();

const month = new Intl.DateTimeFormat("pt-BR", {

month: "short",

})

.format(date)

.toLocaleUpperCase()

.replace(".", "");

  

return { day, month };

}

  

function formatDateHeading(inputDate: string | Date) {

const date = new Date(inputDate);

const formattedDate = new Intl.DateTimeFormat("pt-BR", {

year: "numeric",

month: "long",

}).format(date);

return formattedDate

.replace(/^\w/, (c) => c.toUpperCase())

.replace("de", " ");

}

  

function ShouldRender({ children, when }) {

return when ? children : null;

}

  

export function InstallmentsList() {

const isDesktop = useMediaQuery("lg");

const tokenDrawer = useUrlDisclosure("token");

const navigate = useNavigate();

const { data } = useUser();

const { recurrenceId, parcel } = useParams() as {

recurrenceId: string;

parcel: string;

};

const { data: receiptDetails } = useRecurrenceDetails({

recurrenceId,

config: {

enabled: !!recurrenceId,

suspense: true,

},

});

  

const [currentInstallment, setcurrentInstallment] = useState<number | null>(

null,

);

const [detailDrawerOpen, setDetailDrawerOpen] = useState(false);

const [installmentToDisplay, setInstallmentToDisplay] = useState<string>("");

  

const groupedInstallment = groupBy(receiptDetails?.items, "schedulingDate");

  

// const dispatchToken = Hooks.useDispatchTokenEvent({

// detail: {

// eventIdentifier: "REQUEST_TOKEN",

// handleDate: true,

// request: {

// guid_user: data?.login?.guid_user,

// guid_token_type: "C21F982C-EF61-4802-A95E-2C896B0D56AB",

// path: "",

// },

// },

// });

  

const handleCancelInstallment = () => {

// TODO: implement

};

  

const handlePauseInstallment = () => {

tokenDrawer.open();

};

  

const handleSelectInstallment = (installment: RecurrenceItem) => {

navigate(`detalhes/${installment.installmentNumber}`);

};

  

const shouldShowOutlet =

window.location.pathname.includes(`detalhes/${parcel}`) && isDesktop;

  

const shouldShowOutletDrawer =

window.location.pathname.includes(`detalhes/${parcel}`) && isDesktop;

return (

<>

<ShouldRender when={isDesktop}>

<Styles.Container>

<Styles.Content>

{Object.keys(groupedInstallment).map((key) => {

return (

<Fragment key={key}>

<Styles.Heading>{formatDateHeading(key)}</Styles.Heading>

{groupedInstallment[key].map((installment) => {

const { day, month } = formatDateToInstallmentDisplay(

new Date(installment.schedulingDate),

);

  

return (

<Styles.InstallmentCard

role="link"

key={installment.guidTransferRecurrence}

onClick={() => handleSelectInstallment(installment)}

>

<Styles.LeftContainer>

<Styles.InstallmentDateContainer>

<Styles.InstallmentDay>{day}</Styles.InstallmentDay>

<Styles.InstallmentMonth>

{month}

</Styles.InstallmentMonth>

</Styles.InstallmentDateContainer>

<div>

<Styles.InstallmentStatus>

{`Recorrência ${getRecurrenceStatusByNumber(

installment.status,

receiptDetails.approved,

).toLowerCase()}`}

</Styles.InstallmentStatus>

<Styles.InstallmentReceiver>

{receiptDetails.beneficiaryName}

</Styles.InstallmentReceiver>

</div>

</Styles.LeftContainer>

<div>

<Styles.InstallmentInfo>

{`Parcela ${installment.installmentNumber} de ${receiptDetails.totalInstallments}`}

</Styles.InstallmentInfo>

<Styles.InstallmentAmount>

{formatCurrency(receiptDetails.amount)}

</Styles.InstallmentAmount>

</div>

</Styles.InstallmentCard>

);

})}

</Fragment>

);

})}

{Object.keys(groupedInstallment).length <= 0 && (

<Styles.NoInstallments>

<Styles.NoInstallmentsItem>

<p>Nenhum dado encontrado, tente selecionar outros filtros</p>

</Styles.NoInstallmentsItem>

</Styles.NoInstallments>

)}

</Styles.Content>

<Styles.ButtonContainer>

<Styles.CancelButton

variant="link"

onClick={handleCancelInstallment}

>

Cancelar

</Styles.CancelButton>

<Styles.PauseButton

variant="secondary"

onClick={handlePauseInstallment}

>

{getRecurrenceStatusByNumber(

receiptDetails.status,

receiptDetails.approved,

) === "Pausado"

? "Retomar"

: "Pausar"}

</Styles.PauseButton>

</Styles.ButtonContainer>

</Styles.Container>

<Drawer

open={window.location.pathname.includes(`detalhes/${parcel}`)}

onDismiss={() => navigate(`./`)}

title={`Detalhes da ${parcel || ""}ª parcela`}

position="right"

>

<Outlet />

</Drawer>

</ShouldRender>

<ShouldRender when={!isDesktop}>

<Outlet />

</ShouldRender>

{/* {isDesktop ? (

<Styles.Container>

<Styles.Content>

{Object.keys(groupedInstallment).map((key) => {

return (

<Fragment key={key}>

<Styles.Heading>{formatDateHeading(key)}</Styles.Heading>

{groupedInstallment[key].map((installment) => {

const { day, month } = formatDateToInstallmentDisplay(

new Date(installment.schedulingDate),

);

  

return (

<Styles.InstallmentCard

role="link"

key={installment.guidTransferRecurrence}

onClick={() => handleSelectInstallment(installment)}

>

<Styles.LeftContainer>

<Styles.InstallmentDateContainer>

<Styles.InstallmentDay>{day}</Styles.InstallmentDay>

<Styles.InstallmentMonth>

{month}

</Styles.InstallmentMonth>

</Styles.InstallmentDateContainer>

<div>

<Styles.InstallmentStatus>

{`Recorrência ${getRecurrenceStatusByNumber(

installment.status,

receiptDetails.approved,

).toLowerCase()}`}

</Styles.InstallmentStatus>

<Styles.InstallmentReceiver>

{receiptDetails.beneficiaryName}

</Styles.InstallmentReceiver>

</div>

</Styles.LeftContainer>

<div>

<Styles.InstallmentInfo>

{`Parcela ${installment.installmentNumber} de ${receiptDetails.totalInstallments}`}

</Styles.InstallmentInfo>

<Styles.InstallmentAmount>

{formatCurrency(receiptDetails.amount)}

</Styles.InstallmentAmount>

</div>

</Styles.InstallmentCard>

);

})}

</Fragment>

);

})}

{Object.keys(groupedInstallment).length <= 0 && (

<Styles.NoInstallments>

<Styles.NoInstallmentsItem>

<p>Nenhum dado encontrado, tente selecionar outros filtros</p>

</Styles.NoInstallmentsItem>

</Styles.NoInstallments>

)}

</Styles.Content>

<Styles.ButtonContainer>

<Styles.CancelButton

variant="link"

onClick={handleCancelInstallment}

>

Cancelar

</Styles.CancelButton>

<Styles.PauseButton

variant="secondary"

onClick={handlePauseInstallment}

>

{getRecurrenceStatusByNumber(

receiptDetails.status,

receiptDetails.approved,

) === "Pausado"

? "Retomar"

: "Pausar"}

</Styles.PauseButton>

</Styles.ButtonContainer>

</Styles.Container>

) : (

<Outlet />

)} */}

{/* <ShouldRender when={shouldShowOutletDrawer}> */}

  

{/* </ShouldRender> */}

{/* {

isDesktop ? (

<Drawer

open={window.location.pathname.includes(`detalhes/${parcel}`)}

onDismiss={() => navigate(`./`)}

title={`Detalhes da ${parcel || ""}ª parcela`}

position="right"

>

<Outlet />

</Drawer>

) : null

// <Outlet />

} */}

{/* {tokenDrawer.isOpen && (

<Drawer

open

onDismiss={tokenDrawer.close}

title="Token"

position="right"

>

<Parcel

// eslint-disable-next-line @typescript-eslint/ban-ts-comment

// @ts-ignore

config={() => System.import("@hyperlocal/banking-token-validation")}

mountParcel={mountRootParcel}

// parcelDidMount={dispatchToken}

/>

</Drawer>

)}

{detailDrawerOpen && (

<Drawer

open

onDismiss={() => setDetailDrawerOpen(false)}

title={`Detalhes da ${currentInstallment || ""}ª parcela`}

position="right"

>

<Details

installment={installmentToDisplay}

goToFutureInstallments={() => setDetailDrawerOpen(false)}

/>

</Drawer>

)} */}

</>

);

}

  

function Skeleton() {

return (

<Styles.Container>

<Styles.Content>

{Array.from({ length: 7 }).map((_, key) => {

return (

<Fragment key={key}>

<Styles.Heading>

<VitalSkeleton width="150px" height="20px" />

</Styles.Heading>

{Array.from({ length: 5 }).map((_, index) => {

return (

<Styles.InstallmentCard key={index}>

<Styles.LeftContainer>

<Styles.InstallmentDateContainer>

<Styles.InstallmentDay>

<VitalSkeleton width="30px" height="24px" />

</Styles.InstallmentDay>

<Styles.InstallmentMonth>

<VitalSkeleton width="30px" height="24px" />

</Styles.InstallmentMonth>

</Styles.InstallmentDateContainer>

<div>

<Styles.InstallmentStatus>

<VitalSkeleton width="250px" height="20px" />

</Styles.InstallmentStatus>

<Styles.InstallmentReceiver>

<VitalSkeleton width="250px" height="20px" />

</Styles.InstallmentReceiver>

</div>

</Styles.LeftContainer>

<div>

<Styles.InstallmentInfo>

<VitalSkeleton width="100px" height="20px" />

</Styles.InstallmentInfo>

<Styles.InstallmentAmount>

<VitalSkeleton width="100px" height="20px" />

</Styles.InstallmentAmount>

</div>

</Styles.InstallmentCard>

);

})}

</Fragment>

);

})}

</Styles.Content>

</Styles.Container>

);

}

  

InstallmentsList.Skeleton = Skeleton;
```